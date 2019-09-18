# Configuring Master/Slave Replication with Connector/J
## application.yml
```
jdbc:mysql:replication://[master host][:port],[slave host 1][:port][,[slave host 2][:port]]...[/[database]] 
```

## readOnly
```kotlin
@Transactional(readOnly = true)
fun findEmployee(id: Int): Employee = employeeRepository.findById(id)
```

## update, insert, delete
```kotlin
@Transactional(readOnly = true)
```
↑をつけない