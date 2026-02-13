# Week 4: Lists & In-Memory CRUD - Code Snippets

## 1. Basic FlatList - Displaying a Simple List

```javascript
import { View, Text, FlatList, StyleSheet } from 'react-native';

export default function App() {
  // Hardcoded data to start
  const tasks = [
    { id: '1', title: 'Learn React Native' },
    { id: '2', title: 'Build a mobile app' },
    { id: '3', title: 'Practice every day' }
  ];

  // Function to render each item
  const renderTask = ({ item }) => {
    return (
      <View style={styles.taskItem}>
        <Text style={styles.taskText}>{item.title}</Text>
      </View>
    );
  };

  return (
    <View style={styles.container}>
      <FlatList
        data={tasks}
        renderItem={renderTask}
        keyExtractor={(item) => item.id}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  taskItem: {
    padding: 15,
    backgroundColor: '#f0f0f0',
    borderRadius: 8,
    marginBottom: 10,
  },
  taskText: {
    fontSize: 16,
  }
});
```

**Key Points:**
- `data` prop receives the array to display
- `renderItem` is called for each item in the array
- `keyExtractor` tells FlatList how to identify each item uniquely

---

## 2. CREATE - Adding Items to the List

```javascript
import { useState } from 'react';
import { View, Text, TextInput, Button, FlatList, StyleSheet } from 'react-native';

export default function App() {
  const [tasks, setTasks] = useState([
    { id: '1', title: 'Learn React Native' },
    { id: '2', title: 'Build a mobile app' }
  ]);
  const [inputText, setInputText] = useState('');

  const handleAddTask = () => {
    if (inputText.trim() === '') {
      alert('Please enter a task!');
      return;
    }

    // Create new task object
    const newTask = {
      id: Date.now().toString(), // Simple unique ID
      title: inputText
    };

    // Add to the list using spread operator
    setTasks([...tasks, newTask]);
    
    // Clear the input
    setInputText('');
  };

  const renderTask = ({ item }) => {
    return (
      <View style={styles.taskItem}>
        <Text style={styles.taskText}>{item.title}</Text>
      </View>
    );
  };

  return (
    <View style={styles.container}>
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          placeholder="Enter a new task"
          value={inputText}
          onChangeText={setInputText}
        />
        <Button title="Add" onPress={handleAddTask} />
      </View>

      <FlatList
        data={tasks}
        renderItem={renderTask}
        keyExtractor={(item) => item.id}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  inputContainer: {
    flexDirection: 'row',
    marginBottom: 20,
    gap: 10,
  },
  input: {
    flex: 1,
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 10,
    borderRadius: 5,
  },
  taskItem: {
    padding: 15,
    backgroundColor: '#f0f0f0',
    borderRadius: 8,
    marginBottom: 10,
  },
  taskText: {
    fontSize: 16,
  }
});
```

**Key Points:**
- Use `useState` to manage both the list and input value
- `Date.now()` creates a simple unique ID
- Spread operator `[...tasks, newTask]` creates a new array with the new item
- Always validate input before adding

---

## 3. DELETE - Removing Items from the List

```javascript
import { useState } from 'react';
import { View, Text, TextInput, Button, FlatList, TouchableOpacity, StyleSheet } from 'react-native';

export default function App() {
  const [tasks, setTasks] = useState([
    { id: '1', title: 'Learn React Native' },
    { id: '2', title: 'Build a mobile app' },
    { id: '3', title: 'Practice every day' }
  ]);
  const [inputText, setInputText] = useState('');

  const handleAddTask = () => {
    if (inputText.trim() === '') {
      alert('Please enter a task!');
      return;
    }

    const newTask = {
      id: Date.now().toString(),
      title: inputText
    };

    setTasks([...tasks, newTask]);
    setInputText('');
  };

  const handleDeleteTask = (taskId) => {
    // Filter out the task with matching ID
    setTasks(tasks.filter(task => task.id !== taskId));
  };

  const renderTask = ({ item }) => {
    return (
      <View style={styles.taskItem}>
        <Text style={styles.taskText}>{item.title}</Text>
        <TouchableOpacity 
          style={styles.deleteButton}
          onPress={() => handleDeleteTask(item.id)}
        >
          <Text style={styles.deleteText}>Delete</Text>
        </TouchableOpacity>
      </View>
    );
  };

  return (
    <View style={styles.container}>
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          placeholder="Enter a new task"
          value={inputText}
          onChangeText={setInputText}
        />
        <Button title="Add" onPress={handleAddTask} />
      </View>

      <FlatList
        data={tasks}
        renderItem={renderTask}
        keyExtractor={(item) => item.id}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  inputContainer: {
    flexDirection: 'row',
    marginBottom: 20,
    gap: 10,
  },
  input: {
    flex: 1,
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 10,
    borderRadius: 5,
  },
  taskItem: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 15,
    backgroundColor: '#f0f0f0',
    borderRadius: 8,
    marginBottom: 10,
  },
  taskText: {
    fontSize: 16,
    flex: 1,
  },
  deleteButton: {
    backgroundColor: '#ff4444',
    padding: 8,
    borderRadius: 5,
  },
  deleteText: {
    color: 'white',
    fontWeight: 'bold',
  }
});
```

**Key Points:**
- `.filter()` creates a new array without the deleted item
- Pass the item's ID to the delete function
- Use `TouchableOpacity` for pressable elements (better than Button for inline actions)

---

## 4. UPDATE - Editing Items in the List

```javascript
import { useState } from 'react';
import { View, Text, TextInput, Button, FlatList, TouchableOpacity, Modal, StyleSheet } from 'react-native';

export default function App() {
  const [tasks, setTasks] = useState([
    { id: '1', title: 'Learn React Native' },
    { id: '2', title: 'Build a mobile app' }
  ]);
  const [inputText, setInputText] = useState('');
  
  // State for editing
  const [isEditing, setIsEditing] = useState(false);
  const [editingTask, setEditingTask] = useState(null);
  const [editText, setEditText] = useState('');

  const handleAddTask = () => {
    if (inputText.trim() === '') {
      alert('Please enter a task!');
      return;
    }

    const newTask = {
      id: Date.now().toString(),
      title: inputText
    };

    setTasks([...tasks, newTask]);
    setInputText('');
  };

  const handleDeleteTask = (taskId) => {
    setTasks(tasks.filter(task => task.id !== taskId));
  };

  const startEditing = (task) => {
    setEditingTask(task);
    setEditText(task.title);
    setIsEditing(true);
  };

  const handleSaveEdit = () => {
    if (editText.trim() === '') {
      alert('Task cannot be empty!');
      return;
    }

    // Map through tasks and update the matching one
    setTasks(tasks.map(task => 
      task.id === editingTask.id 
        ? { ...task, title: editText }
        : task
    ));

    // Close modal and reset state
    setIsEditing(false);
    setEditingTask(null);
    setEditText('');
  };

  const handleCancelEdit = () => {
    setIsEditing(false);
    setEditingTask(null);
    setEditText('');
  };

  const renderTask = ({ item }) => {
    return (
      <View style={styles.taskItem}>
        <Text style={styles.taskText}>{item.title}</Text>
        <View style={styles.buttonContainer}>
          <TouchableOpacity 
            style={styles.editButton}
            onPress={() => startEditing(item)}
          >
            <Text style={styles.buttonText}>Edit</Text>
          </TouchableOpacity>
          <TouchableOpacity 
            style={styles.deleteButton}
            onPress={() => handleDeleteTask(item.id)}
          >
            <Text style={styles.buttonText}>Delete</Text>
          </TouchableOpacity>
        </View>
      </View>
    );
  };

  return (
    <View style={styles.container}>
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          placeholder="Enter a new task"
          value={inputText}
          onChangeText={setInputText}
        />
        <Button title="Add" onPress={handleAddTask} />
      </View>

      <FlatList
        data={tasks}
        renderItem={renderTask}
        keyExtractor={(item) => item.id}
      />

      {/* Edit Modal */}
      <Modal
        visible={isEditing}
        animationType="slide"
        transparent={true}
      >
        <View style={styles.modalContainer}>
          <View style={styles.modalContent}>
            <Text style={styles.modalTitle}>Edit Task</Text>
            <TextInput
              style={styles.input}
              value={editText}
              onChangeText={setEditText}
            />
            <View style={styles.modalButtons}>
              <Button title="Cancel" onPress={handleCancelEdit} color="#999" />
              <Button title="Save" onPress={handleSaveEdit} />
            </View>
          </View>
        </View>
      </Modal>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  inputContainer: {
    flexDirection: 'row',
    marginBottom: 20,
    gap: 10,
  },
  input: {
    flex: 1,
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 10,
    borderRadius: 5,
  },
  taskItem: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 15,
    backgroundColor: '#f0f0f0',
    borderRadius: 8,
    marginBottom: 10,
  },
  taskText: {
    fontSize: 16,
    flex: 1,
  },
  buttonContainer: {
    flexDirection: 'row',
    gap: 8,
  },
  editButton: {
    backgroundColor: '#4CAF50',
    padding: 8,
    borderRadius: 5,
  },
  deleteButton: {
    backgroundColor: '#ff4444',
    padding: 8,
    borderRadius: 5,
  },
  buttonText: {
    color: 'white',
    fontWeight: 'bold',
  },
  modalContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: 'rgba(0, 0, 0, 0.5)',
  },
  modalContent: {
    width: '80%',
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 10,
  },
  modalTitle: {
    fontSize: 20,
    fontWeight: 'bold',
    marginBottom: 15,
  },
  modalButtons: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginTop: 20,
  }
});
```

**Key Points:**
- `.map()` creates a new array with the updated item
- Use Modal component for a nice editing experience
- Ternary operator checks if current task matches the one being edited
- Spread operator `{ ...task, title: editText }` updates only the title field

---

## 5. Complete CRUD Example - All Together

```javascript
import { useState } from 'react';
import { 
  View, Text, TextInput, Button, FlatList, 
  TouchableOpacity, Modal, StyleSheet, SafeAreaView 
} from 'react-native';

export default function App() {
  // Main state
  const [tasks, setTasks] = useState([]);
  const [inputText, setInputText] = useState('');
  
  // Edit state
  const [isEditing, setIsEditing] = useState(false);
  const [editingTask, setEditingTask] = useState(null);
  const [editText, setEditText] = useState('');

  // CREATE
  const handleAddTask = () => {
    if (inputText.trim() === '') {
      alert('Please enter a task!');
      return;
    }

    const newTask = {
      id: Date.now().toString(),
      title: inputText,
      completed: false
    };

    setTasks([...tasks, newTask]);
    setInputText('');
  };

  // DELETE
  const handleDeleteTask = (taskId) => {
    setTasks(tasks.filter(task => task.id !== taskId));
  };

  // UPDATE - Start editing
  const startEditing = (task) => {
    setEditingTask(task);
    setEditText(task.title);
    setIsEditing(true);
  };

  // UPDATE - Save changes
  const handleSaveEdit = () => {
    if (editText.trim() === '') {
      alert('Task cannot be empty!');
      return;
    }

    setTasks(tasks.map(task => 
      task.id === editingTask.id 
        ? { ...task, title: editText }
        : task
    ));

    setIsEditing(false);
    setEditingTask(null);
    setEditText('');
  };

  const handleCancelEdit = () => {
    setIsEditing(false);
    setEditingTask(null);
    setEditText('');
  };

  // Toggle completion (bonus feature!)
  const toggleComplete = (taskId) => {
    setTasks(tasks.map(task => 
      task.id === taskId 
        ? { ...task, completed: !task.completed }
        : task
    ));
  };

  // Render each task
  const renderTask = ({ item }) => {
    return (
      <View style={styles.taskItem}>
        <TouchableOpacity 
          style={styles.taskContent}
          onPress={() => toggleComplete(item.id)}
        >
          <Text style={[
            styles.taskText,
            item.completed && styles.completedText
          ]}>
            {item.title}
          </Text>
        </TouchableOpacity>
        
        <View style={styles.buttonContainer}>
          <TouchableOpacity 
            style={styles.editButton}
            onPress={() => startEditing(item)}
          >
            <Text style={styles.buttonText}>✏️</Text>
          </TouchableOpacity>
          <TouchableOpacity 
            style={styles.deleteButton}
            onPress={() => handleDeleteTask(item.id)}
          >
            <Text style={styles.buttonText}>🗑️</Text>
          </TouchableOpacity>
        </View>
      </View>
    );
  };

  return (
    <SafeAreaView style={styles.container}>
      <Text style={styles.header}>My Task List</Text>
      
      {/* Add Task Input */}
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          placeholder="Enter a new task"
          value={inputText}
          onChangeText={setInputText}
          onSubmitEditing={handleAddTask}
        />
        <Button title="Add" onPress={handleAddTask} />
      </View>

      {/* Task List */}
      <FlatList
        data={tasks}
        renderItem={renderTask}
        keyExtractor={(item) => item.id}
        ListEmptyComponent={
          <Text style={styles.emptyText}>No tasks yet. Add one above!</Text>
        }
      />

      {/* Edit Modal */}
      <Modal
        visible={isEditing}
        animationType="slide"
        transparent={true}
      >
        <View style={styles.modalContainer}>
          <View style={styles.modalContent}>
            <Text style={styles.modalTitle}>Edit Task</Text>
            <TextInput
              style={styles.input}
              value={editText}
              onChangeText={setEditText}
              autoFocus
            />
            <View style={styles.modalButtons}>
              <Button title="Cancel" onPress={handleCancelEdit} color="#999" />
              <Button title="Save" onPress={handleSaveEdit} />
            </View>
          </View>
        </View>
      </Modal>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  header: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
  inputContainer: {
    flexDirection: 'row',
    marginBottom: 20,
    gap: 10,
  },
  input: {
    flex: 1,
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 10,
    borderRadius: 5,
    fontSize: 16,
  },
  taskItem: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 15,
    backgroundColor: '#f9f9f9',
    borderRadius: 8,
    marginBottom: 10,
    borderWidth: 1,
    borderColor: '#e0e0e0',
  },
  taskContent: {
    flex: 1,
  },
  taskText: {
    fontSize: 16,
  },
  completedText: {
    textDecorationLine: 'line-through',
    color: '#999',
  },
  buttonContainer: {
    flexDirection: 'row',
    gap: 8,
  },
  editButton: {
    backgroundColor: '#4CAF50',
    padding: 8,
    borderRadius: 5,
    minWidth: 40,
    alignItems: 'center',
  },
  deleteButton: {
    backgroundColor: '#ff4444',
    padding: 8,
    borderRadius: 5,
    minWidth: 40,
    alignItems: 'center',
  },
  buttonText: {
    fontSize: 16,
  },
  emptyText: {
    textAlign: 'center',
    color: '#999',
    marginTop: 50,
    fontSize: 16,
  },
  modalContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: 'rgba(0, 0, 0, 0.5)',
  },
  modalContent: {
    width: '80%',
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 10,
    elevation: 5,
  },
  modalTitle: {
    fontSize: 20,
    fontWeight: 'bold',
    marginBottom: 15,
  },
  modalButtons: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginTop: 20,
  }
});
```

---

## Key Concepts Summary

### FlatList Props
- `data`: Array of items to display
- `renderItem`: Function that renders each item
- `keyExtractor`: Function to get unique key for each item
- `ListEmptyComponent`: What to show when list is empty

### State Management Pattern
```javascript
const [items, setItems] = useState([]); // Always use state for dynamic lists
```

### Array Methods for CRUD
- **Create**: `setItems([...items, newItem])`
- **Read**: Handled by FlatList
- **Update**: `setItems(items.map(item => condition ? updatedItem : item))`
- **Delete**: `setItems(items.filter(item => item.id !== idToDelete))`

### Unique IDs
```javascript
id: Date.now().toString() // Simple approach for beginners
// Later you can use libraries like 'uuid'
```

### Common Pitfalls to Mention
1. Forgetting to use `keyExtractor` (causes warnings)
2. Mutating state directly instead of creating new arrays
3. Not validating input before adding items
4. Forgetting to clear input after adding
