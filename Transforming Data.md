Data can be transformed before storing it into the cache of the endpoint
In the slice

```
    fetchReportsTemplates: builder.query({
      query: () => GET_REPORT_TEMPLATES,
      transformResponse: (response) => getTransformed()
      transformErrorResponse: ({statusCode, data}) => getTransformedErr()
    }),
```


Mutations can also have transformResponse, transformErrorResponse