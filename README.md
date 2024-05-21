# Ayesha-1
Author-Ayesha Sultana
import React, { useState } from 'react';
import './App.css';

const App = () => {
  const [items, setItems] = useState([
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' },
    { id: 4, name: 'Item 4' }
  ]);
  const [droppedItems, setDroppedItems] = useState([]);

  const handleDragStart = (e, id) => {
    e.dataTransfer.setData('text/plain', id);
  };

  const handleDrop = (e) => {
    const itemId = e.dataTransfer.getData('text/plain');
    const item = items.find(item => item.id.toString() === itemId);

    if (item) {
      setItems(items.filter(item => item.id.toString() !== itemId));
      setDroppedItems([...droppedItems, item]);
      console.log(Item '${item.name}' dropped into div B);
    }
  };

  const handleRemoveItem = (id) => {
    const item = droppedItems.find(item => item.id === id);

    if (item) {
      setDroppedItems(droppedItems.filter(item => item.id !== id));
      setItems([...items, item]);
      console.log(Item '${item.name}' removed from div B);
    }
  };

  return (
    <div className="container">
      <div className="div-a">
        <h2>Div A</h2>
        <ul>
          {items.map(item => (
            <li key={item.id} draggable onDragStart={(e) => handleDragStart(e, item.id)}>
              {item.name}
            </li>
          ))}
        </ul>
      </div>
      <div className="div-b" onDrop={handleDrop} onDragOver={(e) => e.preventDefault()}>
        <h2>Div B</h2>
        <ul>
          {droppedItems.map(item => (
            <li key={item.id} onClick={() => handleRemoveItem(item.id)}>
              {item.name}
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
};

export default App;
