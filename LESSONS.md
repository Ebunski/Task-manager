# Lessons

`prop.d.tsx`

## useState

```tsx
const [isToggled, setIsToggled] = useState < boolean > false;
const [subtasks, setSubtasks] = useState<Subtask[]>([]);
const [data, setData] = useState<null | DefinedType>(null);
const [data, setData] = useState<DefinedType>({} as DefinedType);
```

## useRef

`const inputRef = useRef<HTMLInputElement | null>(null);`

## useContext

```tsx
// Pass to createContext
interface ThemeContextType {
  darkMode: boolean;
  toggleDarkMode: () => void;
}
const ThemeContext = React.createContext<ThemeContextType | undefined>;

//also define children prop for provider

// define type of the value object
const themeContextValue: ThemeContextType = {
  darkMode,
  toggleDarkMode,
};

return (
  <ThemeContext.Provider value={themeContextValue}>
    {children}
  </ThemeContext.Provider>
);
```

## redux

```tsx
import { rootState } from "@/redux/reduxTypes";
const { nameBoard } = useSelector(
  (rootReducer: rootState) => rootReducer.reducerNameBoard
);
```

```tsx
import { createSlice, PayloadAction } from "@reduxjs/toolkit";
setBoards: (state, action: PayloadAction<BoardDataType>) => {
      state.boards = action.payload.boards;
    },
```

## React Types

### React Component Types

```tsx
React.FC;
React.Component;
React.ComponentType;
React.ReactNode;
```

### Props Types

`React.FC<Props>`

### Event Types

```TSX
React.MouseEvent
React.KeyboardEvent
React.FormEvent
React.ChangeEvent
```

```TSX
React.ReactElement
React.ReactNode
React.ReactNodeArray: Represents an array of React nodes.
React.ReactNodeList: Represents a list of React nodes.
```

### Functions

`closeModal: () => void;`

```jsx
const handleOnDragOver = (e: React.DragEvent<HTMLUListElement>) => {
  e.preventDefault();
};
```

## Generic Props

Components that can work with a variety of data types or structures, allowing you to specify the types of props that a component can accept as a parameter when defining the component.

### No constraints

```tsx
type MyComponentProps<T> = {
  data: T;
};

// The component receives data of type T.
// T extends any means that the parameter T extends the defined type 'any'
const MyComponent = <T>({ data }: MyComponentProps<T>) => {
  return <div>{data}</div>;
};

// Typescript infers the type
<GenericComponent data={42} />
<GenericComponent data={"hello"} />
```

### With constraints

```tsx
interface Item {
  id: number;
  name: string;
}


type Props<T extends Item> {
  items: T[];
    //items is an objects array which each item must have an id and name i.e mapping through an items array
}
```

## Excluding and Extracting Properties

```tsx
type MyComponentProps = {
  prop1: string;
  prop2: number;
  prop3: boolean;
};

type AllowedProps = Exclude<keyof MyComponentProps, "prop1">;
type AllowedProps = Omit<MyComponentProps, "prop1">;
type StringProp = Extract<keyof MyComponentProps, "prop1">;

function MyComponent(props: AllowedProps) {}
```

## Polymorphic component e.g custom button

```tsx
type ButtonProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & React.ComponentProps<T>;

function Button<T>(props: ButtonProps<T>) {
  const { as: Component = 'button', children, ...rest } = props;
  return <Component {...rest}>{children}</Component>;
}

// Usage examples:
<Button as="a" href="/example">Link Button</Button>
<Button as="button" onClick={() => alert('Clicked')}>Click Me</Button>
```

## Extending Types

```tsx
interface Task {
  title: string;
  description: string;
  status: string;
  subtasks: Subtask[];
}

interface Subtask {
  title: string;
  isCompleted: boolean;
}

interface Column {
  name: string;
  tasks: Task[];
}

interface Board {
  name: string;
  columns: Column[];
}

interface BoardDataType {
  boards: Board[];
}

export { BoardDataType };
```
