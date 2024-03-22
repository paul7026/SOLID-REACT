# SOLID-REACT
## Single Responsibility Principle
### Для каждого модуля должно быть определено единственное назначение

![image](https://github.com/paul7026/SOLID-REACT/assets/59816390/71c9c079-3cf8-4b10-96c0-914eb25396b1)


```JSX
//BAD ❌
export default function TodosPage() {
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    const getTodos = async () => {
      const { data } = await axios.get("https://someapi/todos/");
      setTodos(data);
    };

    getTodos();
  }, []);

  return (
    <div>
      <h1>My Todos:</h1>
      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>{`ID: ${todo.id}, Title: ${todo.title}`}</li>
        ))}
      </ul>
    </div>
  );
}
```
