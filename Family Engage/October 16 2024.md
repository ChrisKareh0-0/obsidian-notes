### **Journal of Progress on Fetching Subusers for a Main User**

**Objective**: Implement a frontend and backend solution to fetch and display sub-users associated with a main user, using `userId`.

---

### **Step 1: Backend Setup for Subuser Fetching**

- **Problem**: The original API `/api/subusers` returned a `404` error because the route didn’t exist. You wanted to create a route that fetches sub-users for the logged-in main user.

- **Solution**: We set up an API route `/api/users/subusers` to fetch sub-users for the main user, identified by `userId`.

**Backend API: `/api/users/subusers/route.ts`**:

```typescript
// app/api/users/subusers/route.ts

import { NextResponse } from 'next/server';
import User from 'path_to_your_user_model';  // Replace with the correct import path for your User model

export async function GET(req: Request) {
    const url = new URL(req.url);
    const userId = url.searchParams.get('userId');  // Get the userId from query params

    if (!userId) {
        return NextResponse.json({ error: 'User ID is required' }, { status: 400 });
    }

    try {
        // Find the main user by userId and populate their subUsers
        const user = await User.findById(userId).populate('subUsers').exec();

        if (!user) {
            return NextResponse.json({ error: 'User not found' }, { status: 404 });
        }

        // Return the sub-users
        return NextResponse.json(user.subUsers, { status: 200 });
    } catch (error) {
        return NextResponse.json({ error: 'Failed to fetch sub-users' }, { status: 500 });
    }
}
```

- **Explanation**:
  - The API extracts `userId` from the query parameters.
  - The backend fetches the main user by `userId` and populates the `subUsers` field using MongoDB’s `populate()` function.

---

### **Step 2: Frontend Adjustments to Fetch Subusers**

- **Problem**: The frontend was set to fetch the current user instead of the sub-users for a specific `userId`. Initially, we discussed using JWT, but we decided to drop that and focus on passing the `userId` directly.

- **Solution**: Update the frontend component to pass `userId` to the `getSubUsers` function in `useUserService`.

**Frontend: Updated `Users.tsx`**:

```typescript
'use client';

import { useEffect, useState } from 'react';
import Link from 'next/link';
import { Spinner } from '_components';
import { useUserService } from '_services';

type SubUser = {
    username: string;
    firstName: string;
    lastName: string;
};

export default function Users() {
    const userService = useUserService();
    const [subUsers, setSubUsers] = useState<SubUser[]>([]);  // State to hold sub-users
    const [loading, setLoading] = useState(true);  // Loading state
    const mainUserId = 'someUserId';  // Replace with the actual main user ID

    useEffect(() => {
        const fetchSubUsers = async () => {
            try {
                // Fetch sub-users for the main user (by userId)
                const data: SubUser[] = await userService.getSubUsers(mainUserId);
                setSubUsers(data);
            } catch (error) {
                console.error('Error fetching sub-users:', error);
                setSubUsers([]);
            } finally {
                setLoading(false);  // Stop loading
            }
        };

        fetchSubUsers();  // Call the function to fetch sub-users
    }, [userService, mainUserId]);

    if (loading) {
        return <Spinner />;  // Show spinner while loading
    }

    return (
        <>
            <h1>Family Members</h1>
            <Link href="/users/add" className="btn btn-sm btn-success mb-2">Add Family Member</Link>
            <table className="table table-striped">
                <thead>
                    <tr>
                        <th style={{ width: '30%' }}>First Name</th>
                        <th style={{ width: '30%' }}>Last Name</th>
                        <th style={{ width: '30%' }}>Username</th>
                    </tr>
                </thead>
                <tbody>
                    <TableBody />
                </tbody>
            </table>
        </>
    );

    function TableBody() {
        if (subUsers.length > 0) {
            return subUsers.map((subUser, index) => (
                <tr key={index}>
                    <td>{subUser.firstName}</td>
                    <td>{subUser.lastName}</td>
                    <td>{subUser.username}</td>
                </tr>
            ));
        }

        return (
            <tr>
                <td colSpan={3} className="text-center">
                    <div className="p-2">No Sub Users To Display</div>
                </td>
            </tr>
        );
    }
}
```

- **Explanation**:
  - The `mainUserId` is passed to `userService.getSubUsers(mainUserId)` to fetch the sub-users for the given user.
  - Once fetched, the sub-users are displayed in a table format.

---

### **Step 3: Update `useUserService` to Handle `userId`**

- **Problem**: Initially, `getSubUsers` did not expect a `userId`. However, to fetch sub-users associated with a main user, we need to pass `userId`.

- **Solution**: Update the `getSubUsers` function in `useUserService` to accept `userId` and use it in the request.

**Updated `useUserService.ts`**:

```typescript
function useUserService(): IUserService {
    return {
        // Other methods...

        getSubUsers: async (userId: string) => {
            try {
                // Fetch sub-users by userId
                const response = await fetch(`/api/users/subusers?userId=${userId}`, {
                    method: 'GET',
                    headers: { 'Content-Type': 'application/json' },
                });
                return await response.json();
            } catch (error) {
                throw new Error('Failed to fetch sub-users');
            }
        },

        // Other methods...
    };
}
```

- **Explanation**:
  - `getSubUsers` now takes `userId` as a parameter and makes a `GET` request to `/api/users/subusers?userId=${userId}`.
  - The backend then fetches the sub-users for that `userId`.

---

### **Step 4: Error Handling and Final Adjustments**

- **Problem**: There were additional type errors in TypeScript where `userId` was not passed correctly or was missing.

- **Solution**: 
  - Ensure `userId` is passed explicitly to `getSubUsers`.
  - Improve error handling both on the frontend and backend to catch issues when fetching sub-users.

---

### **Conclusion**

- **Frontend**: The component fetches sub-users for a given `userId` and displays them in a table.
- **Backend**: The API correctly handles the `userId` parameter to fetch sub-users associated with the main user.
- **Next Steps**: Ensure `userId` is dynamically passed based on the logged-in user or context in your application.

---

Let me know if you need further modifications or additional details on any part!