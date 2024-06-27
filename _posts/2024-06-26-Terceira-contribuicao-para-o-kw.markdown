---
layout: post
title:  "Terceira contribuição para o kw"
date:   2024-06-26 19:58:00 -0300
categories: IME-USP MAC0470-Software_Livre
---

Este artigo é uma exploração da minha terceira  experiência contribuindo ao kw ou kworkflow, sendo isso o terceiro projeto na disciplina de Desenvolvimento d>

# Objetivos do projeto

Como a segunda contribuição para o kw foi relativamente pequena, eu e minha dupla decidimos fazer uma outra contribuição, escolhendo um outro issue de maior complexidade.

# Como vou contribuir?

Dessa vez decidimos resolver o [issue 1062][link-issue], que envolvia refator o código do kw_db.sh, que continha diversas funções que mexiam em bases de dados. Fizemos diversas mudanças, adicionando uma funçao run_sql_query para evitar código duplicado, mudamos a ordem dos argumentos em duas funções para evitar que elas fossem chamadas com diversos argumentos vazios, adicionamos testes para a nova função adicionada, adicionamos um código de erro faltante, mudamos a ordem das funções no arquivo que testava as funções de kw_db para ambos os arquivos ficarem com a mesma ordem, e também adicionamos comentários faltantes sobre os argumentos das funções.

# Modificação do código

Aqui segue a função adicionada:

  # This function runs a sql command in a given database and
  # executes a pre command if it is passed. 
  #
  # @query:     SQL query that will be executed
  # @db:        Name of the database file
  # @db_folder: Path to the folder that contains @db
  # @flag:      Flag to control function output
  # @pre_cmd:   Pre command to executed, if passed  
  #
  # Return:
  # 2 if db doesn't exist;
  # 0 if succesful; non-zero otherwise
  function run_sql_query()
  {
    local query="$1"
    local db="${2:-"$DB_NAME"}"
    local db_folder="${3:-"$KW_DATA_DIR"}"
    local flag=${4:-'SILENT'}
    local pre_cmd="$5"
    local cmd
    local db_path

    db_path="$(join_path "$db_folder" "$db")"

    if [[ ! -f "$db_path" ]]; then
      complain 'Database does not exist'
      return 2 # ENOENT
    fi

    if [[ -n "$pre_cmd" ]]; then
      cmd="sqlite3 -init "${KW_DB_DIR}/pre_cmd.sql" -cmd \"${pre_cmd}\" \"${db_path}\" -batch \"${query}\""
    else
      cmd="sqlite3 -init "${KW_DB_DIR}/pre_cmd.sql" \"${db_path}\" -batch \"${query}\""
    fi

    cmd_manager "$flag" "$cmd"
}

E aqui seu código teste:

  function test_execute_sql_script()
  {
    local output
    local expected
    local ret

    output=$(execute_sql_script 'wrong/path/invalid_script.sql')
    ret="$?"
    assert_equals_helper 'Invalid script, error expected' "$LINENO" 2 "$ret"

    output=$(execute_sql_script "$DB_FILES/init.sql")
    ret="$?"
    expected="Creating database: $KW_DATA_DIR/kw.db"
    assert_equals_helper 'No errors expected' "$LINENO" 0 "$ret"
    assert_equals_helper 'DB file does not exist, should warn' "$LINENO" "$expected" "$output"

    assertTrue "($LINENO) DB file should be created" '[[ -f "$KW_DATA_DIR/kw.db" ]]'

    # Here we make use of SQLite's internal commands to return a list of the
    # tables in the db, the semicolon ensures sqlite3 closes
    output=$(sqlite3 "$KW_DATA_DIR/kw.db" -cmd '.tables' -batch ';')
    expected='^fake_table[[:space:]]+pomodoro[[:space:]]+statistics[[:space:]]+tags[[:space:]]*$'
    assertTrue "($LINENO) Testing tables" '[[ "$output" =~ $expected ]]'

    execute_sql_script "$DB_FILES/insert.sql"
    ret="$?"
    assert_equals_helper 'No errors expected' "$LINENO" 0 "$ret"

    # counting the number rows in each table
    output=$(sqlite3 "$KW_DATA_DIR/kw.db" -batch 'SELECT count(id) FROM tags;')
    assert_equals_helper 'Expected 4 tags' "$LINENO" 4 "$output"

    output=$(sqlite3 "$KW_DATA_DIR/kw.db" -batch 'SELECT count(rowid) FROM pomodoro;')
    assert_equals_helper 'Expected 5 pomodoro entries' "$LINENO" 5 "$output"

    output=$(sqlite3 "$KW_DATA_DIR/kw.db" -batch 'SELECT count(rowid) FROM statistics;')
    assert_equals_helper 'Expected 4 statistic entries' "$LINENO" 4 "$output"
  }

# Finalização da contribuição
Depois de várias iterações refazendo as mudanças para que não houvessem mais erros nos testes, finalmente desmarcamos o PR como draft e o deixamos como ativo. Por enquanto está aguardando ser aceito.

[link-issue]: https://github.com/kworkflow/kworkflow/issues/1062
