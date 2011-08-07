{spawn, exec} = require 'child_process'
log = console.log
      
task 'build', ->
  run 'coffee -o lib -c src/*.coffee'
    
task 'bench', ->
  run 'cd benchmarks && ./run'
    
task 'test', ->
  run 'cd tests && expresso *.coffee'
    
task 'docs', ->
  run 'docco src/zappa.coffee'
        
run = (args...) ->
  for a in args
    switch typeof a
      when 'string' then command = a
      when 'object'
        if a instanceof Array then params = a
        else options = a
      when 'function' then callback = a
  
  command += ' ' + params.join ' ' if params?
  cmd = spawn '/bin/sh', ['-c', command], options
  cmd.stdout.on 'data', (data) -> log data.toString()
  cmd.stderr.on 'data', (data) -> log data.toString()
  process.on 'SIGHUP', -> cmd.kill()
  cmd.on 'exit', (code) -> callback() if callback? and code is 0