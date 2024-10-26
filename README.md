# Git-test-demo

This is my first Git repo


https://www.youtube.com/watch?v=XQmM0oF8dhs

https://www.youtube.com/watch?v=2JBx_06dD1k

https://www.youtube.com/watch?v=8vNFuUALYv4

https://www.youtube.com/watch?v=OJ1tkK3mJSw&t=2004s

https://www.youtube.com/results?search_query=crud+operation+with+api+using+rtk+query+piyush+garg


https://www.youtube.com/watch?v=YQD3AgzjwUg






1) Redux
   -----
   Redux is a open source state management library for cross-component or app-wide state. It is used to maintain and update data across our application for multiple components to share. It's used to manage and centralizing application state.
   
2) Redux vs React context
   ----------------------
   i) In more complex apps, using React Context can lead to deeply nested or "fat" "Context provider" components. So it's better to use redux for complex application. (Complex setup & management)

   ii) React context is not optimized for high frequency state changes. So it's better to use redux. (Performance)

   iii) Redux is a state management library while useContext is a hook.

3) How redux works
   ---------------
   Redux is all about having one central data store in our application.
   
   ** Components can't directly manipulate data in the store.
   
  Action: Actions are JavaScript object that contains information. Actions are the only source of information for the store.
  
  Basically it's a object with a type property.
  
  Reducer: Reducers are the pure functions that contain the logic and calculation that needed to be performed on the state.
  
  ** Pure function means that same input always should produce same output and there should be no side effects inside of that fucntion (There shouldn't be any http or local storage request). 
  
  Store: Store is an object which provides the state of the application. This object is accessible with help of the provider in the files of the project.  

       
         View/UI -- dispatch --> Action --> Reducers --> store -- subscribe --> View/UI

  First component dispatch  certain action which is a javascript object describes the kind of operation, the reducer should perform. Therefore Redux forward the action to reducer, reads that description of the desired operation and this operation is performed by reducer and reducer returns a new state to store. Then we can use the new state to component from store.

   ** react-redux package makes connecting react applications to redux stores and reducers ends on very simple way.

   ** useSelector hook is used to access the state that managed by store.

      Ex: useSelector(state=> state.counter)

    we should pass a function to useSelector, which will receive the state managed by redux and then return a part of the state which we want to extract. 

    when we use useSelector, then React Redux will automatically set up a subscription to the Redux store for this component. 

   ** useDispatch hook is used to dispatch an action. When we call useDispatch() function, we don't pass any argument to it but instead, it's give us back a dispatch function which is used to dispatch action. 	
   
   Ex:
   --
   function handleIncrement(){
     dispatch({type: 'increment'})
   }
   
   function handleIncrementByFive(){
     dispatch({type: 'increment', payload: 5})
   }
   
   ** Objects returns in the reducer will not be merged with the existing state. They will overwrite the existing state. So we must update the other state when we update the state in reducer.

   Ex:
   --
  ** redux is not react specific, it can be used in any javascript project and doesn't know about react. To make working with redux in React applications easier, there is a second package which we should install , the react-redux package. 
   
   The react-redux package which makes connecting react applications to redux store and reducers ends on very simple. It makes very simple to subscribe components to the redux store. 
   
   // add example
   
   ** If we have synchronous, side-effect free code , then we should write our logic in reducers and avoid action creators or components.
    
	If we have asynchronous, side-effect code , then we should prefer action creators or components.
	
   what is middleware in redux
   ---------------------------
   Suppose we want to fetch data from API, then we can't perform this operation in reducer as it's a pure function. So here we can use middleware between action and reducer.
   
   So, Middleware is a function which runs between dispatching an action and the moment it reaches the reducer. Here middleware can stop the action to reach reducer function.

   Middleware can be used for various tasks such as logging, crash reporting, performing asynchronous actions, and more.

     dispatch action(action first go to middleware) --> middleware(logger,error handler) --> reducer

   ** middleware function is curried type of function.

   Ex:

   const logger=(store)=>(next)=>(action)=>{
    console.log(store);
    next(action);
   }   
    
   Redux challenges
   ----------------
   ** For large application, it's hard to maintain action type, We could have been clashing indentifiers.
   
   ** its hard to maintain state imutability in reducers. 
   
   
   
4) Redux toolkit
   -------------
   Redux toolkit is a set of tools that helps simplify Redux development. Basically it's a upgraded and simplified version of Redux.
   
   Reasons for preferring RTK(Redux ToolKit):

   i) RTK gives the ability to write mutable state updates in the reducers.

   ii) It also eliminates the use of extra coding by providing boilerplates.

   iii) RTK also has the feature of RTK query which eliminates the use of Thunks and makes the query processing faster

   ** configureStore like createStore creates a store but it's makes merging multiple reducers into one reducer in easier way.
   
   ** In Redux Toolkit, extraReducers is an optional configuration object that allows you to define additional reducers that respond to actions generated by other parts of your application, such as thunks or other slices.
   
   How to add multiple reducers in store 
   -------------------------------------
   Suppsoe there are two slice: counterSlice, authSlice
   
   import { configureStore } from '@reduxjs/toolkit'

   export const store = configureStore({
     reducer: {counter: counterSlice.reducer, auth: authSlice.reducer },
   })
   
   "counter" and "auth", there are  user defined names, we can use any names.
   
   **If reducers export default then we can use any name. 

   ** We should execute side-effects and async tasks in inside the components (e.g. via useEffect()) and inside the action creators. 
   
   ** We will define our action and reducer in Slice.
   
   Flow of Redux toolkit--->
   
   Application --> Action --> Dispatch --> Slice --> State Update --> Store

5) Thunk
   -----
   A Thunk is a function that delays an action until later means until something else finished. Redux Thunk is a middleware that allows you to return functions instead of plain objects and that function can dispatch a new action object.
   
   We could write an action creator as a thunk, which does not immediately return the action object but returns another function which eventually returns the action. So we can run some other code before we then dispatch the actual action object that we did want to create. 
   
   when you need to fetch data from an API, you can’t wait for the API response synchronously in the action creator. Redux Thunk addresses this limitation by allowing action creators to return functions that can perform asynchronous operations.
 
   Ex:
   --
   // Action creator using Redux Thunk
    const fetchData = () => {
     return (dispatch) => {
       dispatch({ type: 'FETCH_REQUEST' });
       fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => dispatch({ type: 'FETCH_SUCCESS', payload: data }))
      .catch(error => dispatch({ type: 'FETCH_FAILURE', payload: error }));
     };
    };
	
	Thunk vs Saga
	-------------
	i) Redux thunk uses functions as action creators that return functions. Redux Saga uses generator functions for managing side effects.
	
	ii) Thunk suitable for simpler use cases and smaller applications, whereas saga suitable for complex asynchronous flows, provides better scalability.
	
6) Redux DevTools
   --------------
  This is an extra tools which we can use to make debugging Redux and our Redux state a bit easier. 

  ** INIT is automatically dispatched first action that applies all our initial states to Redux so that initializes the store.







   














