import logo from './logo.svg';
import './App.css';
import React, {useState} from 'react';
import{ DragDropContext,Droppable,Draggable} from 'react-beautiful-dnd';
import uuid from 'uuid/v4';
 
const itemsFromBackend=[
  {id: uuid(),content:'First task'},
  {id: uuid(),content:'second task'}
];

const columnsFromBackend =
  {
    [uuid()] : {
      name:'Research',
      item: itemsFromBackend
    },
    [uuid()] : {
      name:' To Do',
      item: []
    },
      [uuid()]: {
        name:'On Progress',
        item: [] 
      }
  };
const onDragEnd = (result,column,setColumns) => {
  if(!result.destination) return;
  const { source,destination } = result;
  if(source.droppableId !==destination.dropableId){
    const sourceColumn=columns[source.droppableId];
    const destColumn = columns[destination.droppableId];
    const sourceItem = [...sourceColumn.items];
    const destiItems = [...destColumn.items];
    const [removed] = sourceItems.splice(source.index,1);
    destiItems.splice(destination.index,0,removed);
    setColumns({
      ...columns,
      [source.droppableId]:{
        ...sourceColumn,
        items:sourceItems
      },
      [destination.droppableId]:{
        ...destColumn,
        items:destItems
      }
    })
  }else{
  const column = columns[source.droppableId];
  const copiedItems = [...column.items]
  const [removed] = copiedItems.splice(source.index,1);
  copiedItems.splice(dextination.index,0, removed);
  setColumns({
    ...columns,
    [source.droppableId]: {
      ...column,
      items: copiedItems
    }
  });
};
function App() {
  const[columns, setColumns] = useState(columnsFromBackend);
  return (
    <div style={{display:'flex',justifyContent:'center',height:'100%'}}>
    <DragDropContext onDragEnd={result => onDragEnd(result, columns, setColumns)}>
    {Object.entries(columns).man(([id,column]) =>{
      return(
        <div style={{display:'flex',flexDirection:'columns',alignItems:'center'}}>
        <h2>{column.name}</h2>
        <div style={{margin:8}}
        <Droppable droppableId={id} key = {id}>
        {(provided,snapshot) =>{
          return(
            <div 
            {...provided.droppableProps}
            ref={provided.innerRef}
            style={{
              background:snapshot.isDraggingOver? 'Lightblue' : 'Lightgrey',
              padding: 4,
              width:250,
              minHeight:500
            }}
            >
            {column.items.map((item,index) =>{
              return(
                <Draggable key={item.id} draggbleId={item.id} index={index}>
                {(provided, snapshot) =>{
                  return(
                    <div 
                    ref={provided.innerRef}
                    {...provided.draggableProps}
                    {...provided.dragHandleProps}
                    style={{
                      userSelect:'none',
                      padding:16,
                      margin:'0 0 8px 0',
                      minHeight:'50px',
                      backgroundColor:snapshot.isDragging ? '#2634A' : '#456C86',
                      color:'white',
                      ...provided.draggableProps.style
                    }}
                    >
                    </div>
                  )
                }}
                </Draggable>
              )
            })}
            </div>
          )
        }
        </Droppable>
        </div>
        </div>
      )
    }
    </DragDropContext>
    </div>
  );
}

export default App;
