
API slice objects will have endpoints field insaide them. We can use them to trigger data fetches and read cached data for that endpoints


**Initiating requests**

reportsSlice

```
const {data, isLoading, isSuccess ...} = await dispatch(reportsSlice.endpoints.getPost.initiate(arg, options))

// Can be simplified, export the endpoints from the slice and you can do 
await dispatch(getPost.initite(arg, options))
```

However, doing it this way will initiate a **subscription**, if you do not unsubscribe, rtk query will  no know when to clear the cached data

```
const promise = dipatch(xyz.initiate(arg, options))
const {...} = await promise
...do stuff with data
promise.unsubscribe()
```

Better still, if we do not need a subscription at all, 
pass in options: 
```
For Queries:
{subscribe: false}
For mutations: 
{track: false}


dispatch(reportsSlice.endpoints.getPost.initiate(arg, {subscribe: false}))
```
**Selecting data**

Use endpoint.select()

