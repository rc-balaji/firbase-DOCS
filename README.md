# Firebase Authentication and Firestore Documentation

This README document provides a comprehensive guide to using Firebase Authentication and Firestore in your web applications. It covers user authentication processes, including sign-up, sign-in, sign-out, and user profile management. Additionally, it details how to interact with Firestore for data storage, retrieval, and real-time updates, along with implementing security rules.

## Table of Contents

- [Authentication](#authentication)
  - [Initializing Authentication](#initializing-authentication)
  - [Creating a New User](#creating-a-new-user)
  - [Email Verification](#email-verification)
  - [Signing In](#signing-in)
  - [Signing Out](#signing-out)
  - [Auth State Changes](#auth-state-changes)
  - [Google Sign-In](#google-sign-in)
  - [Updating User Profiles](#updating-user-profiles)
  - [Getting Current User](#getting-current-user)
- [Firestore Database](#firestore-database)
  - [Initializing Firestore](#initializing-firestore)
  - [Adding Data](#adding-data)
  - [Creating Custom Documents](#creating-custom-documents)
  - [Server Timestamps](#server-timestamps)
  - [Retrieving Data](#retrieving-data)
  - [Real-time Data](#real-time-data)
- [Security Rules](#security-rules)
  - [Authenticated Users Only](#authenticated-users-only)
  - [User-based Data Access](#user-based-data-access)
- [Queries](#queries)
  - [Querying Documents](#querying-documents)
  - [Ordering Documents](#ordering-documents)
- [Document Updates and Deletion](#document-updates-and-deletion)
  - [Updating Documents](#updating-documents)
  - [Deleting Documents](#deleting-documents)

## Authentication

### Initializing Authentication

```javascript
import { getAuth } from 'fb/auth';
const auth = getAuth(app);
```

### Creating a New User

```javascript
import { createUserWithEmailAndPassword } from 'fb/auth';
createUserWithEmailAndPassword(auth, email, pass).then((userCredential) => {
    const user = userCredential.user;
}).catch((error) => console.error(error));
```

### Email Verification

```javascript
sendEmailVerification(auth.currentUser);
```

### Signing In

```javascript
import { signInWithEmailAndPassword } from '/auth';
signInWithEmailAndPassword(auth, email, password);
```

### Signing Out

```javascript
import { signOut } from '/auth';
signOut(auth);
```

### Auth State Changes

```javascript
import { onAuthStateChanged } from '/auth';
onAuthStateChanged(auth, (user) => {
    if (user) {
        const uid = user.uid;
    } else {
    }
});
```

### Google Sign-In

```javascript
import { GoogleAuthProvider, signInWithPopup } from '/auth';
const provider = new GoogleAuthProvider();
signInWithPopup(auth, provider);
```

### Updating User Profiles

```javascript
import { updateProfile } from '/auth';
updateProfile(auth.currentUser, {
    displayName: "Jane Q. User",
    photoURL: "https://example.com/jane-q-user/profile.jpg"
}).then(() => {
}).catch((error) => {
});
```

### Getting Current User

```javascript
const user = auth.currentUser;
```

## Firestore Database

### Initializing Firestore

```javascript
import { getFirestore } from '/firestore';
const db = getFirestore(app);
```

### Adding Data

```javascript
import { collection, addDoc } from '/firestore';
const coll = collection(db, 'collection-name');
await addDoc(coll, {
    first: "SSS",
});
```

### Creating Custom Documents

```javascript
import { doc, setDoc } from '/firestore';
await setDoc(doc(db, 'collection-name', 'document-name'), {
    first: "SSS",
});
```

### Server Timestamps

```javascript
import { serverTimestamp } from '/firestore';
{
    createdAt: serverTimestamp()
}
```

### Retrieving Data

```javascript
import { getDocs } from '/firestore';
const querySnapshot = await getDocs(coll);
querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
});
```

### Real-time Data

```javascript
import { onSnapshot } from '/firestore';
onSnapshot(coll, (querySnapshot) => {
});
```

## Security Rules

### Authenticated Users Only

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### User-based Data Access

Refer to the provided Firestore rules for scenarios like allowing reads based on user IDs, updating data based on users, and deleting documents based on user authentication.

## Queries

### Querying Documents

```javascript
import { query, where } from '/firestore';
const q = query(coll, where("uid", "==", user.uid));
```

### Ordering Documents

```javascript
import { orderBy } from '/firestore';
const q = query(postsRef, where("uid", "==", user.uid), orderBy("createdAt", "desc"));
```

## Document Updates and Deletion

### Updating Documents

```javascript
import { doc, updateDoc } from '/firestore';
async function updatePostInDB(docId, newBody) {
    const postRef = doc(db, 'collection-name', docId);
    await updateDoc(postRef, { body: newBody });
}
```

### Deleting Documents

```javascript
import { deleteDoc } from '/firebase';
async function deletePostFromDB(docId) {
    await deleteDoc(doc(db, 'collection-name', docId));
}
```

This document should serve as a guide for implementing Firebase Authentication and Firestore in your web applications, ensuring secure and efficient data handling and user management.
