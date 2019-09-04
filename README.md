# bug recreate

## this works as expected

```
fly -t dev execute --config task.yml --load-vars-from vars.yml --input src=.
```

## this fails
