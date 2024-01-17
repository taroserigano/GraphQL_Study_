
TESTING QUERIES:

-to use localhost:5000 for query testing, 

app.use(
  '/graphql',
  graphqlHTTP({
    schema,
    graphiql: process.env.NODE_ENV === 'development', <-- when this is true, you can use localhost:5000 for testing GraphQL queries 
  })
);



GETTING DATA -

-use Queries to get data 

const { loading, error, data } = useQuery(GET_CLIENTS);

-create Queries 

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
    
    export { GET_CLIENTS };


With Parameters: 

    const GET_PROJECT = gql`
      query getProject($id: ID!) {
        project(id: $id) {
          id
          name
          description
          status
          client {
            id
            name
            email
            phone
          }
        }
      }
    `;
    
    const { loading, error, data } = useQuery(GET_PROJECT, { variables: { id } });
    


MODIFICATION / MUTATION 

QUERY - 

    const ADD_CLIENT = gql`
      mutation addClient($name: String!, $email: String!, $phone: String!) {
        addClient(name: $name, email: $email, phone: $phone) {
          id
          name
          email
          phone
        }
      }
    `;

-HOW TO USE 

       const [name, setName] = useState('');
        const [email, setEmail] = useState('');
        const [phone, setPhone] = useState('');
      
        const [addClient] = useMutation(ADD_CLIENT, {
          variables: { name, email, phone },
          update(cache, { data: { addClient } }) {
            const { clients } = cache.readQuery({ query: GET_CLIENTS });
      
            cache.writeQuery({
              query: GET_CLIENTS,
              data: { clients: [...clients, addClient] },
            });
          },
        });
      
        const onSubmit = (e) => {
          e.preventDefault()
      
          addClient(name, email, phone);

        };
      


