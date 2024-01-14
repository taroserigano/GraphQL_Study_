**************** CLIENT 

App.js: 

- How to manage Cache:

  const cache = new InMemoryCache({
  typePolicies: {
    Query: {
      fields: {
        clients: {
          merge(existing, incoming) {
            return incoming;
          },
        },

- merge function tells the cache to accept new incoming data, and allows it to overwrtie existing cache and update.



  
HOW TO CREATE GRAPHQL QUERY - only to get 

const GET_CLIENTS = gql`
  query getClients {
    clients {
      id
      name
      email
      phone
    }
  }
`;

export { GET_CLIENTS } <-- use GET_CLIENTS 


HOW TO CREATE MODIFY METHODS 

const ADD_CLIENT = gql`
  mutation addClient($name: String!, $email: String!, $phone: String!) {
    
    // add parameters 
    addClient(name: $name, email: $email, phone: $phone) {
      id
      name
      email
      phone
    }
  }
`;

// addClient method is created 
  const [addClient] = useMutation(ADD_CLIENT, {

  // define variables 
    variables: { name, email, phone },

    // update cache like below 
    update(cache, { data: { addClient } }) {

    // get existing clients 
      const { clients } = cache.readQuery({ query: GET_CLIENTS });

    // add new Client 
      cache.writeQuery({
        query: GET_CLIENTS,
        data: { clients: [...clients, addClient] },
      });
    },
  








