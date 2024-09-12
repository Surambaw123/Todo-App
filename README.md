{/* #BSIT-3R4 ACTIVITY#1
#Jimoya, Jude Adrianne B., Sebere Michael*/}

import React, { useState } from 'react';
import { StyleSheet, Text, View, TextInput, TouchableOpacity, FlatList, Alert } from 'react-native';

export default function App() {
  const [text, setText] = useState('');
  const [todos, setTodos] = useState([]);
  const [search, setSearch] = useState('');
  const [editTodo, setEditTodo] = useState(null);

  const addTodo = () => {
    if (text.trim().length > 0) {
      if (editTodo) {
        setTodos(todos.map(todo => (todo.id === editTodo.id ? { ...todo, text } : todo)));
        setEditTodo(null);
      } else {
        setTodos([...todos, { id: Math.random().toString(), text }]);
      }
      setText('');
    }
  };

  const removeTodo = (id) => {
    Alert.alert(
      'Delete Todo',
      'Are you sure you want to delete this todo?',
      [
        {
          text: 'Cancel',
          style: 'cancel',
        },
        {
          text: 'Delete',
          onPress: () => setTodos(todos.filter(todo => todo.id !== id)),
        },
      ],
      { cancelable: true }
    );
  };

  const startEditTodo = (todo) => {
    setEditTodo(todo);
    setText(todo.text);
  };

  const filteredTodos = todos.filter(todo => todo.text.toLowerCase().includes(search.toLowerCase()));

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Todo List</Text>
      <TextInput
        style={styles.input}
        placeholder="Search todos"
        value={search}
        onChangeText={setSearch}
      />
      <TextInput
        style={styles.input}
        placeholder={editTodo ? "Edit your todo" : "Add a new todo"}
        value={text}
        onChangeText={setText}
      />
      <TouchableOpacity style={styles.addButton} onPress={addTodo}>
        <Text style={styles.addButtonText}>{editTodo ? 'Update Todo' : 'Add Todo'}</Text>
      </TouchableOpacity>
      <FlatList
        data={filteredTodos}
        renderItem={({ item }) => (
          <View style={styles.todoItem}>
            <Text style={styles.todoText}>{item.text}</Text>
            <View style={styles.todoActions}>
              <TouchableOpacity style={styles.editButton} onPress={() => startEditTodo(item)}>
                <Text style={styles.editText}>Edit</Text>
              </TouchableOpacity>
              <TouchableOpacity style={styles.removeButton} onPress={() => removeTodo(item.id)}>
                <Text style={styles.removeText}>Delete</Text>
              </TouchableOpacity>
            </View>
          </View>
        )}
        keyExtractor={item => item.id}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {

    marginTop: 190,
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  title: {
    fontSize: 30,
    marginTop: 30,
    marginBottom: 20,
    textAlign: 'center',
  },
  input: {
    borderBottomWidth: 1,
    marginBottom: 20,
    paddingHorizontal: 8,
    fontSize: 18,
    borderBottomColor: '#17c1e8',
  },
  addButton: {
    backgroundColor: '#17c1e8',
    padding: 10,
    borderRadius: 25,
    alignItems: 'center',
    marginBottom: 20,
  },
  addButtonText: {
    color: '#fff',
    fontSize: 18,
  },
  todoItem: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    paddingVertical: 10,
    borderBottomWidth: 5,
    borderBottomColor: '#17c1e8',
  },
  todoText: {
    fontSize: 18,
  },
  todoActions: {
    flexDirection: 'row',
  },
  editButton: {
    backgroundColor: '#f39c12',
    padding: 5,
    borderRadius: 10,
    marginRight: 10,
    alignItems: 'center',
  },
  editText: {
    color: 'white',
    fontSize: 16,
  },
  removeButton: {
    backgroundColor: '#e74c3c',
    padding: 5,
    borderRadius: 10,
    alignItems: 'center',
  },
  removeText: {
    color: 'white',
    fontSize: 16,
  },
});
