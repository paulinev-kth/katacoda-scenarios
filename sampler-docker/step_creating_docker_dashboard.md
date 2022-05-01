## Monitoring a docker container
The main ressources monitored are generally
- CPU
- memory
- network
- disk read/rewrtite 
  
These ressouces can be seen with the command `docker stats`, but we will now put them into the config file

## Memory and CPU usage
This sampler component will give you timechart for each of the ressource
```yaml
runcharts:
  - title: CPU Redis container
    position: [[0, 0], [25, 10]]
    rate-ms: 1000
    legend:
        enabled: true
        details: false
    scale: 2
    items:
      - label: CPU in %
        color: 178
        sample: docker stats $containerName --no-stream --format {{.CPUPerc}} | cut -d '%' -f 1
  - title: Memory Redis container
    position: [[0, 10], [25, 10]]
    rate-ms: 1000
    legend:
        enabled: true
        details: false
    scale: 2
    items:
      - label: Memory in %
        color: 78
        sample: docker stats $containerName --no-stream --format {{.MemPerc}} | cut -d '%' -f 1
```
{{copy}}

This config defined the two runchars. We have aldready seen the label 'title' and 'sample'. The label 'position' defines where the component should be in the dashboard. The place and size of a component can also be changed with the arrows one the dashboard is running. The label 'rate-ms' gives the frequency of the update of the data. 
So the shell command `docker stats $containerName --no-stream --format {{.CPUPerc}} | cut -d '%' -f 1` is based on the shell command seen above. We add the `--no-stream` property as sampler takes care of the update. The last part `--format {{.CPUPerc}} | cut -d '%' -f 1` enables to filter the data to keep only the CPU percentage and, to split that text with the separator "%", and to keep the only first part as sampler is expecting a number and not a percentage. 

## Global vision of all our containers
On the previous chart, we focused only on one container, but we can also want to have the vision of all our ressources used by the containers.


```yaml
textboxes:
  - title: CPU usage
    rate-ms: 500
    position: [[25, 0], [15, 10]]
    sample: echo Container $containerName && docker stats $containerName --no-stream --format {{.CPUPerc}} && echo '\nAll containers' && docker stats --no-stream --format {{.CPUPerc}} | awk '{sum += $0} END {print sum"%"}'
  - title: Memory usage
    rate-ms: 500
    position: [[25, 10], [15, 10]]
    sample: echo Container $containerName && docker stats $containerName --no-stream  --format {{.MemPerc}} && echo '\nAll containers' && docker stats --no-stream --format {{.MemPerc}} | awk '{sum += $0} END {print sum"%"}'
```
{{copy}}
`echo Container $containerName && docker stats $containerName --no-stream --format {{.CPUPerc}} && echo '\nAll containers' && docker stats --no-stream --format {{.CPUPerc}} | awk '{sum += $0} END {print sum"%"}'`
This shell command concatenates different command
 - `echo Container $containerName` to display the text
 - `docker stats $containerName --no-stream --format {{.CPUPerc}}` is similar to the one seen above
 - `docker stats --no-stream --format {{.CPUPerc}} | awk '{sum += $0} END {print sum"%"}'` sums the CPU usage of all the containers


With these components, you can see your running containers and the stopped containers. 
```yaml
textboxes:
  - title: Docker stats
    position: [[0, 20], [60, 15]]
    sample: docker stats --no-stream
  - title: Running containers
    rate-ms: 500
    position: [[40, 0], [20, 10]]
    sample: docker container ls | awk '{sum += 1} END {print sum-1}' && echo && docker container ls --format "table {{.ID}}\t{{.Image}}"
  - title: Stopped containers
    rate-ms: 500
    position: [[40, 10], [20, 10]]
    sample: docker ps --filter "status=exited" | awk '{sum += 1} END {print sum-1}' && echo && docker ps --filter "status=exited" --format "table {{.ID}}\t{{.Image}}"
```
{{execute}}


## Printing the Redis data
As we can exectute any shell command, we can create more complicated component
This one looked at the data in our redis store. One should pay attention that the CPU usage increase everytime, the updated are fetched

```yaml
textboxes:
- title: Redis Data
    position: [[60, 0], [20, 5]]
    sample: echo Keys/Values in $containerName
    border: true
  - title: Keys
    rate-ms: 10000
    position: [[60, 5], [10, 30]]
    sample: docker exec $containerName redis-cli keys \*
  - title: Values
    rate-ms: 10000
    position: [[70, 5], [10, 30]]
    sample: docker exec $containerName redis-cli keys \* | while read line ; do docker exec $containerName redis-cli get $line ; done
```

You can try to add key value to your container in another terminl and see the data changed.
`docker exec myFirstRedisContainer redis-cli set apple 50kr`{{execute}}

