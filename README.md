# SOLID-REACT
## Single Responsibility Principle
### Для каждого модуля должно быть определено единственное назначение

![image](https://github.com/paul7026/SOLID-REACT/assets/59816390/71c9c079-3cf8-4b10-96c0-914eb25396b1)

BAD ❌
```JSX
interface Todo {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
}

export default function TodosPage() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const getTodos = async () => {
      try {
        const { data } = await axios.get(
          "https://jsonplaceholder.typicode.com/todos"
        );
        setTodos(data);
      } catch (error) {
        if (error instanceof Error) setError(error.message);
      } finally {
        setIsLoading(false);
      }
    };

    getTodos();
  }, []);

  if (isLoading) return <p>Loading...</p>;

  if (error) return <p>Error: {error}</p>;

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
\
&nbsp;
GOOD ✅
```JSX
//getTodos.ts
export const getTodos = async () => {
  try {
    const { data } = await axios.get(
      "https://jsonplaceholder.typicode.com/todos"
    );
    return data;
  } catch (error) {
    if (error instanceof Error) throw new Error(error.message);
  }
};
```
```JSX
//useTodos.tsx
export const useTodos = () => {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    getTodos()
      .then((data) => setTodos(data))
      .catch((error) => {
        setError(error.message);
      })
      .finally(() => setIsLoading(false));
  }, []);

  return { todos, isLoading, error };
};
```
```JSX
//TodoItem.tsx
export const TodoItem = ({ id, title }: TodoItemProps) => {
  return <li>{`ID: ${id}, Title: ${title}`}</li>;
};
```
```JSX
//TodoList.tsx
export const TodoList = ({ todos }: TodoListProps) => {
  return (
    <ul>
      {todos.map(({ id, title }) => (
        <TodoItem key={id} title={title} id={id} />
      ))}
    </ul>
  );
};
```
```JSX
//TodosPage.tsx
export default function TodosPage() {
  const { todos, isLoading, error } = useTodos();

  if (isLoading) return <p>Loading...</p>;

  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h1>My Todos:</h1>
      <TodoList todos={todos} />
    </div>
  );
}
```
:white_check_mark:  Читаемость кода\
:white_check_mark:  Проще тестировать\
:white_check_mark:  Проще поддерживать\
:white_check_mark:  Проще дорабатывать
