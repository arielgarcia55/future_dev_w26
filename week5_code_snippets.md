# Week 5: Data Persistence with AsyncStorage
## Code Snippets

---

## 1. Installation & Import

```bash
# Install AsyncStorage
npx expo install @react-native-async-storage/async-storage
```

```javascript
// Import at the top of any file that needs it
import AsyncStorage from '@react-native-async-storage/async-storage';
```

---

## 2. Saving Data (setItem)

```javascript
// Saving a simple string
const saveUsername = async (username) => {
  try {
    await AsyncStorage.setItem('username', username);
    console.log('Username saved!');
  } catch (error) {
    console.error('Error saving username:', error);
  }
};

// Saving an object (must be converted to a string first)
const saveUser = async (user) => {
  try {
    const jsonValue = JSON.stringify(user);
    await AsyncStorage.setItem('user', jsonValue);
    console.log('User saved!');
  } catch (error) {
    console.error('Error saving user:', error);
  }
};

// Example usage
saveUsername('alex123');
saveUser({ name: 'Alex', age: 25, hobby: 'coding' });
```

---

## 3. Loading Data (getItem)

```javascript
// Loading a simple string
const loadUsername = async () => {
  try {
    const username = await AsyncStorage.getItem('username');
    if (username !== null) {
      console.log('Loaded username:', username);
    }
  } catch (error) {
    console.error('Error loading username:', error);
  }
};

// Loading an object (must be parsed back from string)
const loadUser = async () => {
  try {
    const jsonValue = await AsyncStorage.getItem('user');
    const user = jsonValue != null ? JSON.parse(jsonValue) : null;
    console.log('Loaded user:', user);
    return user;
  } catch (error) {
    console.error('Error loading user:', error);
  }
};
```

---

## 4. Deleting Data (removeItem)

```javascript
// Remove a single item
const deleteUsername = async () => {
  try {
    await AsyncStorage.removeItem('username');
    console.log('Username deleted!');
  } catch (error) {
    console.error('Error deleting username:', error);
  }
};

// Clear ALL stored data (use with caution!)
const clearAll = async () => {
  try {
    await AsyncStorage.clear();
    console.log('All data cleared!');
  } catch (error) {
    console.error('Error clearing storage:', error);
  }
};
```

---

## 5. Understanding async/await

```javascript
// ❌ Without async/await — data won't be ready in time
const badExample = () => {
  const data = AsyncStorage.getItem('key'); // This is a Promise, not the data!
  console.log(data); // Prints: Promise { <pending> }
};

// ✅ With async/await — waits for data before continuing
const goodExample = async () => {
  const data = await AsyncStorage.getItem('key'); // Waits until data is ready
  console.log(data); // Prints the actual value
};
```

---

## 6. Loading Data When the App Starts (useEffect)

```javascript
import React, { useState, useEffect } from 'react';
import { View, Text } from 'react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';

export default function App() {
  const [items, setItems] = useState([]);

  // This runs automatically when the screen loads
  useEffect(() => {
    loadItems();
  }, []); // The empty [] means "run once on startup"

  const loadItems = async () => {
    try {
      const jsonValue = await AsyncStorage.getItem('my-items');
      if (jsonValue !== null) {
        setItems(JSON.parse(jsonValue));
      }
    } catch (error) {
      console.error('Error loading items:', error);
    }
  };

  return (
    <View>
      <Text>Items loaded: {items.length}</Text>
    </View>
  );
}
```

---

## 7. Saving a List of Items

```javascript
// Save the whole list every time it changes
const saveItems = async (itemsToSave) => {
  try {
    const jsonValue = JSON.stringify(itemsToSave);
    await AsyncStorage.setItem('my-items', jsonValue);
  } catch (error) {
    console.error('Error saving items:', error);
  }
};

// Add a new item and save
const addItem = async (newItem) => {
  const updatedItems = [...items, newItem];
  setItems(updatedItems);
  await saveItems(updatedItems); // Save after updating
};

// Delete an item and save
const deleteItem = async (id) => {
  const updatedItems = items.filter(item => item.id !== id);
  setItems(updatedItems);
  await saveItems(updatedItems); // Save after updating
};
```

---

## 8. Putting It All Together — Full Example

```javascript
import React, { useState, useEffect } from 'react';
import { View, Text, TextInput, Button, FlatList, StyleSheet } from 'react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';

const STORAGE_KEY = 'my-items'; // Store the key in a constant to avoid typos

export default function App() {
  const [items, setItems] = useState([]);
  const [inputText, setInputText] = useState('');

  // Load data when app starts
  useEffect(() => {
    loadItems();
  }, []);

  const loadItems = async () => {
    try {
      const jsonValue = await AsyncStorage.getItem(STORAGE_KEY);
      if (jsonValue !== null) {
        setItems(JSON.parse(jsonValue));
      }
    } catch (error) {
      console.error('Error loading:', error);
    }
  };

  const saveItems = async (updatedItems) => {
    try {
      await AsyncStorage.setItem(STORAGE_KEY, JSON.stringify(updatedItems));
    } catch (error) {
      console.error('Error saving:', error);
    }
  };

  const addItem = async () => {
    if (inputText.trim() === '') return;
    const newItem = { id: Date.now().toString(), text: inputText };
    const updatedItems = [...items, newItem];
    setItems(updatedItems);
    await saveItems(updatedItems);
    setInputText('');
  };

  const deleteItem = async (id) => {
    const updatedItems = items.filter(item => item.id !== id);
    setItems(updatedItems);
    await saveItems(updatedItems);
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>My Persistent List</Text>
      <TextInput
        style={styles.input}
        value={inputText}
        onChangeText={setInputText}
        placeholder="Add something..."
      />
      <Button title="Add Item" onPress={addItem} />
      <FlatList
        data={items}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.item}>
            <Text>{item.text}</Text>
            <Button title="Delete" onPress={() => deleteItem(item.id)} />
          </View>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20, paddingTop: 60 },
  title: { fontSize: 24, fontWeight: 'bold', marginBottom: 20 },
  input: { borderWidth: 1, borderColor: '#ccc', padding: 10, marginBottom: 10, borderRadius: 8 },
  item: { flexDirection: 'row', justifyContent: 'space-between', alignItems: 'center', paddingVertical: 10, borderBottomWidth: 1, borderBottomColor: '#eee' },
});
```

---

## Key Concepts Summary

| Concept | What it does |
|---|---|
| `AsyncStorage.setItem(key, value)` | Saves data to the device |
| `AsyncStorage.getItem(key)` | Loads data from the device |
| `AsyncStorage.removeItem(key)` | Deletes one item |
| `AsyncStorage.clear()` | Deletes everything |
| `JSON.stringify()` | Converts object → string (for saving) |
| `JSON.parse()` | Converts string → object (for loading) |
| `async/await` | Waits for slow operations to finish |
| `useEffect(() => {}, [])` | Runs code once when the screen loads |
