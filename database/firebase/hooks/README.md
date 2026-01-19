# Firebase React Hooks Templates

Custom React hooks for Firebase integration with TypeScript support.

## Author

{{AUTHOR}}

## Files

### 1. use-auth.ts.tmpl

Firebase Authentication hook with comprehensive auth features:

**Features:**
- Email/password sign in and sign up
- OAuth providers: Google, GitHub
- Password reset functionality
- User profile updates
- Real-time auth state listener
- Loading and error states
- TypeScript User type

**Usage:**
```typescript
const {
  user,
  loading,
  error,
  signInWithEmail,
  signUpWithEmail,
  signInWithGoogle,
  signInWithGitHub,
  signOut,
  resetPassword,
  updateUserProfile
} = useAuth();
```

### 2. use-firestore.ts.tmpl

Firestore hooks for real-time and one-time data operations:

**Hooks included:**
- `useDocument<T>()` - Real-time single document
- `useCollection<T>()` - Real-time collection with query support
- `useAddDocument<T>()` - Add document mutation
- `useUpdateDocument<T>()` - Update document mutation
- `useDeleteDocument()` - Delete document mutation
- `useOptimisticUpdate<T>()` - Optimistic updates with rollback
- `useFetchDocument<T>()` - One-time document fetch
- `useFetchCollection<T>()` - One-time collection fetch

**Features:**
- TypeScript generics for type safety
- Automatic subscription cleanup
- Loading and error states
- Query constraints support
- Optimistic updates with rollback

**Usage:**
```typescript
// Real-time document
const { data, loading, error } = useDocument<User>('users', userId);

// Real-time collection with query
const { data, loading, error } = useCollection<Post>(
  'posts',
  [where('published', '==', true), orderBy('createdAt', 'desc')]
);

// Mutations
const { addDocument } = useAddDocument<Post>('posts');
const { updateDocument } = useUpdateDocument<Post>('posts');
const { deleteDocument } = useDeleteDocument('posts');
```

### 3. use-storage.ts.tmpl

Firebase Storage hook with upload progress and file management:

**Hooks included:**
- `useStorage()` - File upload with progress tracking
- `useDownloadURL()` - Get download URL for a file
- `useDeleteFile()` - Delete file from storage
- `useListFiles()` - List files in a path
- `useFileMetadata()` - Get file metadata

**Utility functions:**
- `generateFilePath()` - Generate unique file paths
- `validateFileType()` - Validate file MIME types
- `validateFileSize()` - Validate file size

**Features:**
- Upload progress tracking (0-100%)
- Cancel upload support
- Custom metadata support
- TypeScript typed metadata
- Automatic cleanup

**Usage:**
```typescript
// Upload with progress
const { uploadFile, uploadState, cancelUpload } = useStorage();

await uploadFile(file, `images/${userId}/${file.name}`, {
  uploadedBy: userId,
  description: 'Profile picture'
});

console.log(uploadState.progress); // 0-100
console.log(uploadState.downloadURL); // URL when complete

// Get download URL
const { url, loading, error } = useDownloadURL('images/user1/photo.jpg');

// Delete file
const { deleteFile } = useDeleteFile();
await deleteFile('images/user1/old-photo.jpg');

// List files
const { files, loading, refresh } = useListFiles('images/user1');
```

## Requirements

- Firebase SDK v9+
- React 18+
- TypeScript with strict mode
- `firebase-config.ts` file exporting initialized Firebase instances

## Firebase Config

These hooks expect a `firebase-config.ts` file in the parent directory with:

```typescript
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';
import { getFirestore } from 'firebase/firestore';
import { getStorage } from 'firebase/storage';

const firebaseConfig = {
  // Your config
};

const app = initializeApp(firebaseConfig);

export const auth = getAuth(app);
export const db = getFirestore(app);
export const storage = getStorage(app);
```

## Best Practices

1. **Type Safety**: Always provide TypeScript generics for document/collection hooks
2. **Cleanup**: All hooks automatically cleanup subscriptions on unmount
3. **Error Handling**: Always handle errors from mutation hooks
4. **Optimistic Updates**: Use for better UX, but handle rollback on errors
5. **File Validation**: Always validate file type and size before upload

## Dependencies

```json
{
  "dependencies": {
    "firebase": "^10.x.x",
    "react": "^18.x.x"
  },
  "devDependencies": {
    "@types/react": "^18.x.x",
    "typescript": "^5.x.x"
  }
}
```

## License

Part of {{PROJECT_NAME}} - All rights reserved
