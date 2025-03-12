**Reference** https://redux-toolkit.js.org/rtk-query/usage/cache-behavior
**Queries**
- Cache keys are a combination of endpoint + argument (optional)
- If argument is an object, it will deepcompare and check if they are equal - refer example below, First timeout will cause a refetch , second timeout will not, even though object references for payload changes

```
const [payload, setPayload] = useState({ reportId: 1004 });
const { data: test } = useFetchWithPostQuery({ reportId, payload });
  
useEffect(() => {
		setTimeout(() => {
		const newPayload = { reportId: 1003 };
		setPayload(newPayload);
	}, 1000);

setTimeout(() => {
		const newPayload = { reportId: 1004 };
		setPayload(newPayload);
	}, 1000);
}, []);



```

**Clearing Cache**
- No subscribers, 
	- Subscribers: Components which are currently hooked onto the cache
	- If there are no subscribers, by default cleared in 60s. This can be overridden with **keepUnusedDataFor : INFINITY
- Can be cleared using tags
