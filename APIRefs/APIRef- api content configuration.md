**Important ones**
All

```
endpoints: (builder) => ({  
	updateUser: builder.query({  
		query: (args) => ({  
			url: ,  
			params: ,
			method: ,  
			body: , // Body is automatically converted to json with the correct headers
			headers: {
			},
			//by default, fetchBaseQuery assumes every response is json, this can be overriden
			responseHandler: (res) => res.text()
			//handing non-default response codes- by default, any non 2xx code is rejected, sent to error, but this can be overridden, return isError
			validateStatus:(response, result) => {
			} 
			timeout:
			
		}),  
}),
```


query can also simply be

query: (arg) => ENDPOINT
