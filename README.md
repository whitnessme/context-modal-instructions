# How to make Easy Modals Using React Context!

![Modal examples](https://miro.medium.com/max/1400/1*VCl3Dz_jwsKOXUjLeD19VA.png)

> This README is refactored & boiled down instructions based on aA Open's *Week 15 > Redux and authentication > Start Authenticate Me Part 2 Frontend bonus instructions!* It details making a modal specifically for Login and Signup--but you can just change the names for directories & variables to make any modal. 

----

## Creating the Context & Setup
**1. Create `context` folder in `src` folder**
```
src
├── components
├── context
├── images
└── store
```
**2. Within the `context` folder, make a `Modal.js` containing:**
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

**3. Make a `Modal.css` with:**
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
*File Structure:*
```
src
├── ...
├── context
│   ├── Modal.css
│   └── Modal.js
└── ...
```
  
**4. In the root directory, `src`, in `index.js`:**
`import { ModalProvider } from './context/Modal';`
And then wrap App component with it like this: 
```js
      <ModalProvider>
        <App />
      </ModalProvider>
```
-----

## Creating the Actual Modal (using Login & Signup as examples)
**5. Create `LoginFormModal` and `SignupFormModal` directories both with  `index.js`'s inside.**
```
src
├── components
│   ├── ...
│   ├── LoginFormModal
│   │   └── index.js
│   ├── SignupFormModal
│   │   └── index.js
│   └── ...
└── ...
```
**6. Put this code inside each `index.js` & for Signup, change variable names:** 
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
*(remember to do the same for **any modal** you want, but change the variables!)*

**7. Put the modal component where you need the button that opens the modal to be. For example:**
This is in my NavBar component, in `NavBar.js` or could be named `index.js` in yours.
```
src
├── components
│   ├── ...
│   ├── NavBar
│   │   └── NavBar.css
│   │   └── NavBar.js
│   └── ...
└── ...
```
```
<ul className='nav-right-side'>
    {!user ?
    <>
        // code removed for brevity
        <li>
          <LoginFormModal />
        </li>
        <li>
          <SignupFormModal />
        </li>
    </>
    :
    // code removed for brevity
```
**8. *(Optional)* Add the setShowModal(false) to your onSubmit functions, inside the forms, if they don't redirect you to a new page or something.**

[Back to top ⬆]()
