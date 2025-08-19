### kf-1
its for coding round 

# Excercise 1 - Code review

## Code Review comments for the UI component.

1. Missing type, "any" should be replaced with proper type. 

   `const [user, setUser] = useState<User|null>(null)`

2. Also while getting the API response, type the response.
   
   `then((data:user)=> setUser(data))`

3. In *useEffect* hook, dependency array is missing "userId"
4. Logic for *loading* would work only the first time. When there is a new "userId" passsed, user will not be notified of loading phase. I would rather use another state variable called *loading* to toggle on before *fetch* operation and toggle-off in the finally block.
5. HTML need to fine tunned to include **schematic** tags like *section*, *article* to comply with WCAG guidelines.

# Best practices suggestion

1. In this approach of fetching API data, there is a possibility of displaying older data due to race condition. This happens when call to API gets completed before the older call.  This can be handled using *AbortController* and pass the signal parameter like
   
   `fetch(url, {signal:controller.signal})`

2. Data fetching logic needs to be revamped. *ReactQuery* or similar libraries to be used to help in caching, better error handling. Also we can use Suspense to load the data smoothly.
   
   `<Suspense fallback={}> <DataComponent> </Suspense>`
   
3. Data can be *pre-fetched* using *router-loader*, by using *userLoaderData()* hook the pre-fetched data can be accessed in the component.


# Code Review of Unit Testing

1. Only basic rendering is checked, there are several missing scenarios like
    + Whether the API URL is hitting right.
      
      `jest.spyon` can help to mimic response
    + Whether the fatched data is present in the DOM.
      
      ```
      test("check if user data is fetched and rendered", async()=> {
         render(<UserProfile userId={1212} />
         expect(await screen.findByText('Durai')).toBeInTheDocument();
        expect(screen.getByText('UI Lead')).toBeInTheDocument();
      });
       ```
      + Hanlding of error scenario
      


