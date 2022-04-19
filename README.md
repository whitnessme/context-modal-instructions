# How to make Modals Using React Context

> More concise instructions, based on aA Open's *Week 15 > Redux and authentication > Start Authenticate Me Part 2 Frontend* bonus instructions
### Creating the Context & Setup
1. Create `context` folder in `src` folder
2. Within the `context` folder, make a `Modal.js` containing: 
```js
import React, {useContext, useRef, useState, useEffect} from 'react';
import ReactDOM from 'react-dom';
import './Modal.css';

const ModalContext = React.createContext();

export function ModalProvider({ children }) {
    const modalRef = useRef();
    const [value, setValue] = useState();


    useEffect(() => {
        setValue(modalRef.current);
    }, [])

    return (
        <>
          <ModalContext.Provider value={value}>
            {children}
          </ModalContext.Provider>
          <div ref={modalRef} />
        </>
    );
}

export function Modal({ onClose, children }) {
    const modalNode = useContext(ModalContext);
    if (!modalNode) return null;
  
    return ReactDOM.createPortal(
      <div id="modal">
        <div id="modal-background" onClick={onClose} />
        <div id="modal-content">
          {children}
        </div>
      </div>,
      modalNode
    );
  }
```

3. Make a `Modal.css` with:
```js
#modal {
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  #modal-background {
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.7);
  }
  
  #modal-content {
    position: absolute;
    background-color:white;
    border-radius: 24px;
  }
```
  
4. In the root directory, in `index.js`: 
`import { ModalProvider } from './context/Modal';`
And then wrap App component with it like this: 
```js
      <ModalProvider>
        <App />
      </ModalProvider>
```
### Creating the Actual Modal (using Login & Signup as examples)
5. Create `LoginFormModal` folder and `SignupFormModal` both with  `index.js`'s inside. 
6. Put this inside the Login one & change variables for the Signup: 
```js
import React, { useState } from 'react';
import { Modal } from '../../context/Modal';
import LoginForm from '../auth/LoginForm';

function LoginFormModal() {
  const [showModal, setShowModal] = useState(false);

  return (
    <>
      <button onClick={() => setShowModal(true)}>Log In</button>
      {showModal && (
        <Modal onClose={() => setShowModal(false)}>
          <LoginForm setShowModal={setShowModal}/>
        </Modal>
      )}
    </>
  );
}

export default LoginFormModal;
```
*(remember to do the same for the SignupFormModal or any other kind of modal you want, but change the variables!)*

7. *(Optional)* Add the setShowModal(false) to your submit buttons inside the forms if they don't redirect you to a new page or something.
