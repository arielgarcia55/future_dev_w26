# Week 3: Navigation and Basic States - Code Snippets

## Part 1: State Management Basics

### 1. Simple useState Example - Counter

```javascript
import { useState } from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';

export default function CounterScreen() {
  // Declare a state variable called 'count' with initial value of 0
  const [count, setCount] = useState(0);

  return (
    <View style={styles.container}>
      <Text style={styles.countText}>Count: {count}</Text>
      
      <View style={styles.buttonContainer}>
        <Button title="Increase" onPress={() => setCount(count + 1)} />
        <Button title="Decrease" onPress={() => setCount(count - 1)} />
        <Button title="Reset" onPress={() => setCount(0)} />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
  },
  countText: {
    fontSize: 32,
    fontWeight: 'bold',
    marginBottom: 30,
  },
  buttonContainer: {
    gap: 10,
    width: '100%',
  }
});
```

**Key Points:**
- `useState` returns two things: the current value and a function to update it
- Always use the setter function (setCount) to update state - never modify directly
- Component re-renders when state changes

---

### 2. TextInput with State - Real-time Input Handling

```javascript
import { useState } from 'react';
import { View, Text, TextInput, StyleSheet } from 'react-native';

export default function InputExample() {
  const [name, setName] = useState('');

  return (
    <View style={styles.container}>
      <Text style={styles.label}>What's your name?</Text>
      
      <TextInput
        style={styles.input}
        placeholder="Enter your name"
        value={name}
        onChangeText={setName}
      />

      <Text style={styles.greeting}>
        {name ? `Hello, ${name}!` : 'Enter your name above'}
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    justifyContent: 'center',
  },
  label: {
    fontSize: 18,
    marginBottom: 10,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 12,
    borderRadius: 5,
    fontSize: 16,
    marginBottom: 20,
  },
  greeting: {
    fontSize: 20,
    color: '#4CAF50',
    textAlign: 'center',
  }
});
```

**Key Points:**
- `value={name}` makes the TextInput controlled by state
- `onChangeText={setName}` updates state as user types
- The component updates in real-time as the user types

---

### 3. Simple Form with Validation

```javascript
import { useState } from 'react';
import { View, Text, TextInput, Button, Alert, StyleSheet } from 'react-native';

export default function SimpleForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleSubmit = () => {
    // Validation
    if (name.trim() === '') {
      Alert.alert('Error', 'Please enter your name');
      return;
    }

    if (email.trim() === '') {
      Alert.alert('Error', 'Please enter your email');
      return;
    }

    if (!email.includes('@')) {
      Alert.alert('Error', 'Please enter a valid email');
      return;
    }

    // If validation passes
    Alert.alert('Success', `Welcome ${name}!`);
    
    // Clear the form
    setName('');
    setEmail('');
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Sign Up Form</Text>

      <TextInput
        style={styles.input}
        placeholder="Name"
        value={name}
        onChangeText={setName}
      />

      <TextInput
        style={styles.input}
        placeholder="Email"
        value={email}
        onChangeText={setEmail}
        keyboardType="email-address"
        autoCapitalize="none"
      />

      <Button title="Submit" onPress={handleSubmit} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    justifyContent: 'center',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 30,
    textAlign: 'center',
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 12,
    borderRadius: 5,
    fontSize: 16,
    marginBottom: 15,
  }
});
```

**Key Points:**
- Always validate user input before processing
- `Alert.alert()` is great for simple feedback
- Clear form after successful submission
- Use appropriate keyboard types for different inputs

---

## Part 2: React Navigation Setup

### 4. Installing Navigation (Terminal Commands)

```bash
# Install React Navigation
npm install @react-navigation/native

# Install dependencies for Expo
npx expo install react-native-screens react-native-safe-area-context

# Install Stack Navigator
npm install @react-navigation/native-stack
```

---

### 5. Basic Stack Navigator Setup

```javascript
// App.js
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import HomeScreen from './screens/HomeScreen';
import DetailsScreen from './screens/DetailsScreen';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen 
          name="Home" 
          component={HomeScreen}
          options={{ title: 'Welcome' }}
        />
        <Stack.Screen 
          name="Details" 
          component={DetailsScreen}
          options={{ title: 'Details Page' }}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

**Key Points:**
- `NavigationContainer` wraps your entire app
- `Stack.Navigator` contains all your screens
- `Stack.Screen` defines each screen with a unique name
- The first screen in the navigator is the initial screen

---

### 6. Basic Navigation Between Screens

```javascript
// screens/HomeScreen.js
import { View, Text, Button, StyleSheet } from 'react-native';

export default function HomeScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  }
});
```

```javascript
// screens/DetailsScreen.js
import { View, Text, Button, StyleSheet } from 'react-native';

export default function DetailsScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Details Screen</Text>
      <Button
        title="Go Back"
        onPress={() => navigation.goBack()}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  }
});
```

**Key Points:**
- Every screen component receives a `navigation` prop automatically
- `navigation.navigate('ScreenName')` goes to a specific screen
- `navigation.goBack()` returns to the previous screen
- React Navigation handles the back button automatically

---

### 7. Passing Parameters Between Screens

```javascript
// screens/HomeScreen.js
import { View, Text, Button, TextInput, StyleSheet } from 'react-native';
import { useState } from 'react';

export default function HomeScreen({ navigation }) {
  const [itemName, setItemName] = useState('');

  const goToDetails = () => {
    if (itemName.trim() === '') {
      alert('Please enter an item name');
      return;
    }

    navigation.navigate('Details', {
      itemName: itemName,
      itemId: Date.now(),
    });
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Home Screen</Text>
      
      <TextInput
        style={styles.input}
        placeholder="Enter item name"
        value={itemName}
        onChangeText={setItemName}
      />

      <Button title="View Details" onPress={goToDetails} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 10,
    borderRadius: 5,
    width: '100%',
    marginBottom: 20,
  }
});
```

```javascript
// screens/DetailsScreen.js
import { View, Text, Button, StyleSheet } from 'react-native';

export default function DetailsScreen({ route, navigation }) {
  // Extract parameters from route
  const { itemName, itemId } = route.params;

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Details Screen</Text>
      
      <View style={styles.infoContainer}>
        <Text style={styles.label}>Item Name:</Text>
        <Text style={styles.value}>{itemName}</Text>
        
        <Text style={styles.label}>Item ID:</Text>
        <Text style={styles.value}>{itemId}</Text>
      </View>

      <Button title="Go Back" onPress={() => navigation.goBack()} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 30,
  },
  infoContainer: {
    width: '100%',
    backgroundColor: '#f5f5f5',
    padding: 20,
    borderRadius: 10,
    marginBottom: 30,
  },
  label: {
    fontSize: 14,
    color: '#666',
    marginTop: 10,
  },
  value: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 10,
  }
});
```

**Key Points:**
- Pass parameters as second argument to `navigate()`
- Access parameters via `route.params` in the destination screen
- You can pass any data: strings, numbers, objects, etc.
- Both `route` and `navigation` props are automatically provided

---

### 8. Navigation with Multiple Screens

```javascript
// App.js
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import HomeScreen from './screens/HomeScreen';
import AddItemScreen from './screens/AddItemScreen';
import ItemDetailsScreen from './screens/ItemDetailsScreen';
import SettingsScreen from './screens/SettingsScreen';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator
        initialRouteName="Home"
        screenOptions={{
          headerStyle: {
            backgroundColor: '#6200ee',
          },
          headerTintColor: '#fff',
          headerTitleStyle: {
            fontWeight: 'bold',
          },
        }}
      >
        <Stack.Screen 
          name="Home" 
          component={HomeScreen}
          options={{ title: 'My App' }}
        />
        <Stack.Screen 
          name="AddItem" 
          component={AddItemScreen}
          options={{ title: 'Add New Item' }}
        />
        <Stack.Screen 
          name="ItemDetails" 
          component={ItemDetailsScreen}
          options={{ title: 'Item Details' }}
        />
        <Stack.Screen 
          name="Settings" 
          component={SettingsScreen}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

```javascript
// screens/HomeScreen.js
import { View, Text, Button, StyleSheet } from 'react-native';

export default function HomeScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Home Screen</Text>
      
      <View style={styles.buttonContainer}>
        <Button
          title="Add Item"
          onPress={() => navigation.navigate('AddItem')}
        />
        <Button
          title="View Item Details"
          onPress={() => navigation.navigate('ItemDetails', {
            itemId: 1,
            itemName: 'Sample Item'
          })}
        />
        <Button
          title="Settings"
          onPress={() => navigation.navigate('Settings')}
          color="#888"
        />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
    marginBottom: 40,
  },
  buttonContainer: {
    width: '100%',
    gap: 15,
  }
});
```

**Key Points:**
- `screenOptions` applies styles to all screens
- `initialRouteName` sets which screen loads first
- Each screen can have individual `options` that override defaults
- Navigation stack automatically adds back buttons

---

### 9. Complete Example: Add Item Flow

```javascript
// screens/AddItemScreen.js
import { useState } from 'react';
import { View, Text, TextInput, Button, StyleSheet, Alert } from 'react-native';

export default function AddItemScreen({ navigation }) {
  const [itemName, setItemName] = useState('');
  const [itemDescription, setItemDescription] = useState('');

  const handleSave = () => {
    if (itemName.trim() === '') {
      Alert.alert('Error', 'Please enter an item name');
      return;
    }

    // In a real app, you'd save this to state or database
    // For now, we'll just navigate to details with the data
    navigation.navigate('ItemDetails', {
      itemName: itemName,
      itemDescription: itemDescription,
      itemId: Date.now(),
      isNew: true,
    });
  };

  return (
    <View style={styles.container}>
      <Text style={styles.label}>Item Name *</Text>
      <TextInput
        style={styles.input}
        placeholder="Enter item name"
        value={itemName}
        onChangeText={setItemName}
      />

      <Text style={styles.label}>Description</Text>
      <TextInput
        style={[styles.input, styles.textArea]}
        placeholder="Enter description (optional)"
        value={itemDescription}
        onChangeText={setItemDescription}
        multiline
        numberOfLines={4}
      />

      <Button title="Save Item" onPress={handleSave} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
  },
  label: {
    fontSize: 16,
    fontWeight: 'bold',
    marginTop: 15,
    marginBottom: 5,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 12,
    borderRadius: 5,
    fontSize: 16,
    marginBottom: 15,
  },
  textArea: {
    height: 100,
    textAlignVertical: 'top',
  }
});
```

```javascript
// screens/ItemDetailsScreen.js
import { View, Text, Button, StyleSheet } from 'react-native';

export default function ItemDetailsScreen({ route, navigation }) {
  const { itemName, itemDescription, itemId, isNew } = route.params;

  return (
    <View style={styles.container}>
      {isNew && (
        <View style={styles.badge}>
          <Text style={styles.badgeText}>✓ Item Created</Text>
        </View>
      )}

      <View style={styles.card}>
        <Text style={styles.label}>Name:</Text>
        <Text style={styles.value}>{itemName}</Text>

        <Text style={styles.label}>Description:</Text>
        <Text style={styles.value}>
          {itemDescription || 'No description provided'}
        </Text>

        <Text style={styles.label}>ID:</Text>
        <Text style={styles.value}>{itemId}</Text>
      </View>

      <View style={styles.buttonContainer}>
        <Button 
          title="Back to Home" 
          onPress={() => navigation.navigate('Home')}
        />
        <Button 
          title="Go Back" 
          onPress={() => navigation.goBack()}
          color="#888"
        />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
  },
  badge: {
    backgroundColor: '#4CAF50',
    padding: 10,
    borderRadius: 5,
    marginBottom: 20,
  },
  badgeText: {
    color: 'white',
    textAlign: 'center',
    fontWeight: 'bold',
  },
  card: {
    backgroundColor: '#f9f9f9',
    padding: 20,
    borderRadius: 10,
    marginBottom: 20,
  },
  label: {
    fontSize: 14,
    color: '#666',
    marginTop: 15,
    marginBottom: 5,
  },
  value: {
    fontSize: 18,
    fontWeight: '500',
  },
  buttonContainer: {
    gap: 10,
  }
});
```

---

## Part 3: Combining State and Navigation

### 10. Practical Example: Task Manager Navigation Flow

```javascript
// App.js
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import TaskListScreen from './screens/TaskListScreen';
import AddTaskScreen from './screens/AddTaskScreen';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen 
          name="TaskList" 
          component={TaskListScreen}
          options={{ title: 'My Tasks' }}
        />
        <Stack.Screen 
          name="AddTask" 
          component={AddTaskScreen}
          options={{ title: 'Add New Task' }}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

```javascript
// screens/TaskListScreen.js
import { useState, useEffect } from 'react';
import { View, Text, Button, FlatList, StyleSheet } from 'react-native';

export default function TaskListScreen({ navigation, route }) {
  const [tasks, setTasks] = useState([
    { id: '1', title: 'Learn React Native', completed: false },
    { id: '2', title: 'Build an app', completed: false },
  ]);

  // Listen for new tasks from AddTaskScreen
  useEffect(() => {
    if (route.params?.newTask) {
      setTasks([...tasks, route.params.newTask]);
      // Clear the parameter
      navigation.setParams({ newTask: undefined });
    }
  }, [route.params?.newTask]);

  const renderTask = ({ item }) => (
    <View style={styles.taskItem}>
      <Text style={styles.taskText}>{item.title}</Text>
    </View>
  );

  return (
    <View style={styles.container}>
      <Button
        title="Add New Task"
        onPress={() => navigation.navigate('AddTask')}
      />

      <FlatList
        data={tasks}
        renderItem={renderTask}
        keyExtractor={item => item.id}
        style={styles.list}
        ListEmptyComponent={
          <Text style={styles.emptyText}>No tasks yet!</Text>
        }
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
  },
  list: {
    marginTop: 20,
  },
  taskItem: {
    padding: 15,
    backgroundColor: '#f0f0f0',
    borderRadius: 8,
    marginBottom: 10,
  },
  taskText: {
    fontSize: 16,
  },
  emptyText: {
    textAlign: 'center',
    color: '#999',
    marginTop: 50,
  }
});
```

```javascript
// screens/AddTaskScreen.js
import { useState } from 'react';
import { View, TextInput, Button, StyleSheet, Alert } from 'react-native';

export default function AddTaskScreen({ navigation }) {
  const [taskTitle, setTaskTitle] = useState('');

  const handleAddTask = () => {
    if (taskTitle.trim() === '') {
      Alert.alert('Error', 'Please enter a task title');
      return;
    }

    const newTask = {
      id: Date.now().toString(),
      title: taskTitle,
      completed: false,
    };

    // Navigate back and pass the new task
    navigation.navigate('TaskList', { newTask });
  };

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.input}
        placeholder="Task title"
        value={taskTitle}
        onChangeText={setTaskTitle}
        autoFocus
      />
      <Button title="Add Task" onPress={handleAddTask} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 12,
    borderRadius: 5,
    fontSize: 16,
    marginBottom: 20,
  }
});
```

---

## Key Concepts Summary

### State Management
```javascript
// Declare state
const [value, setValue] = useState(initialValue);

// Update state
setValue(newValue);

// NEVER do this:
value = newValue; // ❌ Wrong!
```

### Navigation Basics
```javascript
// Navigate to a screen
navigation.navigate('ScreenName');

// Navigate with parameters
navigation.navigate('ScreenName', { param1: value1 });

// Go back
navigation.goBack();

// Access parameters
const { param1 } = route.params;
```

### Common Patterns

**Form Handling:**
1. Create state for each input field
2. Validate before submission
3. Clear form after success

**Navigation Flow:**
1. User fills form
2. Validate input
3. Navigate to next screen with data
4. Display data or confirmation

**Passing Data Back:**
- Navigate back with parameters
- Use `useEffect` to listen for returning data
- Update state when data arrives

### Important Notes
- Each screen component gets `navigation` and `route` props
- State is local to each component (we'll learn about global state later)
- Always validate user input
- Clear sensitive data when navigating away
- Use meaningful screen names for clarity
