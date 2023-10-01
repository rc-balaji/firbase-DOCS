# Firebase and Firestore Operations - Beginner's Guide

This README.md file provides a beginner-friendly guide for common Firebase and Firestore operations using JavaScript. Firebase is a popular platform for building web and mobile applications.

## Prerequisites

Before you begin, make sure you have set up Firebase in your project and have the necessary credentials.

---

## Authentication Operations

### Initialize Firebase Auth

```javascript
import { getAuth } from 'firebase/auth';

const auth = getAuth(app);
Create a New User with Email and Password
javascript
Copy code
import { createUserWithEmailAndPassword } from 'firebase/auth';

// Initialize Firebase Auth
import { getAuth } from 'firebase/auth';
const auth = getAuth(app);

// Replace 'email' and 'password' with the user's email and desired password
createUserWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
    const user = userCredential.user;
    // User successfully created
  })
  .catch((error) => {
    // Handle errors, e.g., invalid email, weak password, etc.
  });
Send Email Verification
javascript
Copy code
// Use sendEmailVerification() on the authenticated user
Sign In with Email and Password
javascript
Copy code
import { signInWithEmailAndPassword } from 'firebase/auth';

// Initialize Firebase Auth
import { getAuth } from 'firebase/auth';
const auth = getAuth(app);

// Replace 'email' and 'password' with the user's email and password
signInWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
    const user = userCredential.user;
    // User successfully signed in
  })
  .catch((error) => {
    // Handle errors, e.g., wrong credentials
  });
Sign Out
javascript
Copy code
import { signOut } from 'firebase/auth';

signOut(auth);
Listen for Authentication State Changes
javascript
Copy code
import { onAuthStateChanged } from 'firebase/auth';

onAuthStateChanged(auth, (user) => {
  if (user) {
    // User is signed in, see Firebase documentation for available properties
    const uid = user.uid;
    // ...
  } else {
    // User is signed out
    // ...
  }
});
Google Sign-In with Popup
javascript
Copy code
import { GoogleAuthProvider, signInWithPopup } from 'firebase/auth';

const provider = new GoogleAuthProvider();

signInWithPopup(auth, provider)
  .then((result) => {
    // Google sign-in successful, get user info from 'result.user'
  })
  .catch((error) => {
    // Handle errors
  });
Update User Profile
javascript
Copy code
import { updateProfile } from 'firebase/auth';

// Update the user's display name and photo URL
updateProfile(auth.currentUser, {
  displayName: "Jane Q. User",
  photoURL: "https://example.com/jane-q-user/profile.jpg"
})
  .then(() => {
    // Profile updated successfully
  })
  .catch((error) => {
    // Handle errors
  });
Get Current User's UID
javascript
Copy code
const user = auth.currentUser;
const uid = user.uid;
Firestore Operations
Initialize Firestore
javascript
Copy code
import { getFirestore } from 'firebase/firestore';

const db = getFirestore(app);
Create a Firestore Collection Reference
javascript
Copy code
import { collection } from 'firebase/firestore';

const coll = collection(db, 'collection-name');
Add a Document to a Collection
javascript
Copy code
import { addDoc } from 'firebase/firestore';

await addDoc(coll, {
  field1: 'Value1',
  field2: 'Value2',
  // ...
});
Create or Update a Document in Firestore
javascript
Copy code
import { doc, setDoc } from 'firebase/firestore';

const docRef = doc(db, 'collection-name', 'document-name');

await setDoc(docRef, {
  field1: 'Value1',
  field2: 'Value2',
  // ...
});
Server Timestamp
javascript
Copy code
import { serverTimestamp } from 'firebase/firestore';

const createdAt = serverTimestamp();
Retrieve Data from Firestore
javascript
Copy code
import { getDocs } from 'firebase/firestore';

const querySnapshot = await getDocs(coll);

querySnapshot.forEach((doc) => {
  console.log(doc.id, " => ", doc.data());
});
Real-time Data Updates with Firestore
javascript
Copy code
import { onSnapshot } from 'firebase/firestore';

onSnapshot(coll, (querySnapshot) => {
  // Handle real-time updates
});
Firestore Security Rules
Here are some example Firestore security rules:

Allow read and write access only to authenticated users:
javascript
Copy code
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
Allow read access based on the user's UID:
javascript
Copy code
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow write: if request.auth != null;
      allow read: if request.auth != null && request.auth.uid == resource.data.uid;
    }
  }
}
Allow read, update, and create access for documents owned by the user:
javascript
Copy code
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    function isSignedIn() {
      return request.auth != null;
    }

    function userIsAuthorOfPost() {
      return request.auth.uid == resource.data.uid;
    }

    match /{document=**} {
      allow create: if isSignedIn();
      allow update: if isSignedIn() && userIsAuthorOfPost();
      allow read: if isSignedIn() && userIsAuthorOfPost();
    }
  }
}
Allow read, update, create, and delete access for documents owned by the user:
javascript
Copy code
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    function isSignedIn() {
      return request.auth != null;
    }

    function userIsAuthorOfPost() {
      return request.auth.uid == resource.data.uid;
    }

    match /{document=**} {
      allow create: if isSignedIn();
      allow read: if isSignedIn() && userIsAuthorOfPost();
      allow update: if isSignedIn() && userIsAuthorOfPost();
      allow delete: if isSignedIn() && userIsAuthorOfPost();
    }
  }
}
Firestore Queries
Basic Query with Where Clause
javascript
Copy code
import { query, where } from 'firebase/firestore';

const q = query(coll, where("uid", "==", user.uid));
Order Documents by a Field
javascript
Copy code
import { query, where, orderBy } from 'firebase/firestore';

const q = query(postsRef, where("uid", "==", user.uid), orderBy("createdAt", "desc"));
Update a Document
javascript
Copy code
import { updateDoc } from 'firebase/firestore';

async function updateDocumentInFirestore(docId, newData) {
  const postRef = doc(db, collectionName, docId);

  await updateDoc(postRef, newData);
}
Delete a Document
javascript
Copy code
import { deleteDoc } from 'firebase/firestore';

async function deleteDocumentInFirestore(docId) {
  await deleteDoc(doc(db, collectionName, docId));
}
