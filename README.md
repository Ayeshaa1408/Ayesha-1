# Ayesha-1
Author-Ayesha Sultana
import React, { useState } from 'react';
import { DndProvider, useDrag, useDrop } from 'react-dnd';
import { HTML5Backend } from 'react-dnd-html5-backend';
import './App.css';

const ItemTypes = {
  ITEM: 'item',
};

const DraggableItem = ({ item, index, moveItem }) => {
  const [{ isDragging }, drag] = useDrag(() => ({
    type: ItemTypes.ITEM,
    item: { item, index },
    collect: (monitor) => ({
      isDragging: !!monitor.isDragging(),
    }),
  }));

  return (
    <div
      ref={drag}
      className="draggable-item"
      style={{ opacity: isDragging ? 0.5 : 1 }}
    >
      {item}
    </div>
  );
};

const DroppableContainer = ({ items, setItems, onDropItem }) => {
  const [{ isOver }, drop] = useDrop(() => ({
    accept: ItemTypes.ITEM,
    drop: (item) => onDropItem(item.item, item.index),
    collect: (monitor) => ({
      isOver: !!monitor.isOver(),
    }),
  }));

  return (
    <div ref={drop} className={`droppable-container ${isOver ? 'highlight' : ''}`}>
      {items.map((item, index) => (
        <DraggableItem key={index} index={index} item={item} moveItem={onDropItem} />
      ))}
    </div>
  );
};

const App = () => {
  const [itemsA, setItemsA] = useState(['Item 1', 'Item 2', 'Item 3']);
  const [itemsB, setItemsB] = useState([]);

  const handleDropA = (item, index) => {
    setItemsB((prevItems) => prevItems.filter((_, i) => i !== index));
    setItemsA((prevItems) => [...prevItems, item]);
    console.log(`Removed ${item} from B and added to A`);
  };

  const handleDropB = (item, index) => {
    setItemsA((prevItems) => prevItems.filter((_, i) => i !== index));
    setItemsB((prevItems) => [...prevItems, item]);
    console.log(`Removed ${item} from A and added to B`);
  };

  return (
    <DndProvider backend={HTML5Backend}>
      <div className="container">
        <div className="column">
          <h2>Div A</h2>
          <DroppableContainer items={itemsA} setItems={setItemsA} onDropItem={handleDropB} />
        </div>
        <div className="column">
          <h2>Div B</h2>
          <DroppableContainer items={itemsB} setItems={setItemsB} onDropItem={handleDropA} />
        </div>
      </div>
    </DndProvider>
  );
};

export default App;
