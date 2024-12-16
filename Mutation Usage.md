

**(1) Simple POST Request**


```
const [updatePost, {data, error,..}] = useUpdatePostMutation()
const handleClick = () => {
	updatePost(postId)
		.unwrap()
		.then(() => dispatch(closeModal()))
		.catch(e => e)
}
```


**(2) POST Request, but you want the data or error or loader or anything else to be populated in a different component**
Use fixed Cache key

```

ComponentA.js: 

const [updatePost, {data, error,..}] = useUpdatePostMutation({fixedCacheKey : "update-post"})

const handleClick = () => {
	updatePost(postId)
}

ComponentB.js:
const [,{error}] = useUpdatePostMutation({fixedCacheKey: "update-post"})

return <KToaster errors={error}/>
```



