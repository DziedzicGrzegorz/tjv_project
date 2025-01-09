# Project Specification

## 1. Description of the Data

The system is a file management(using Azure blob storage) and sharing application, built on a relational database model to support efficient storage, management, and sharing of user files. The main entities in the data model are:

### **1.1. Entities and Relationships**

1. **User**: Represents users of the system. Each user has a unique identifier, username, email, password hash, and timestamps for when the account was created and last updated.
    - **Attributes**:
        - `id`: Unique identifier for each user (Primary Key).
        - `username`: Unique name used for identification.
        - `email`: Unique email address used for communication.
        - `passwordHash`: Encrypted password for authentication.
        - `createdAt`: Timestamp indicating when the user account was created.
        - `updatedAt`: Timestamp indicating the last update to the user account.

2. **File**: Represents files uploaded by users. Each file has metadata like the filename, type, size, and versioning details.
    - **Attributes**:
        - `id`: Unique identifier for each file (Primary Key).
        - `owner`: User who owns the file (Foreign Key referencing `User`).
        - `filename`: Name of the file.
        - `blobUrl`: URL for accessing the file in storage (Azure Blob Storage).
        - `fileType`: Type/MIME type of the file.
        - `size`: Size of the file in bytes.
        - `version`: Version number for versioning purposes.
        - `createdAt`: Timestamp indicating when the file was uploaded.
        - `updatedAt`: Timestamp indicating the last update to the file.

3. **SharedFile**: Represents the sharing of files between users. This entity manages the many-to-many relationship between users and files.
    - **Attributes**:
        - `id`: Unique identifier for each sharing instance (Primary Key).
        - `file`: Reference to the shared file (Foreign Key referencing `File`).
        - `sharedWith`: Reference to the user with whom the file is shared (Foreign Key referencing `User`).
        - `permission`: Defines the access level granted (e.g., "READ").
        - `sharedAt`: Timestamp indicating when the file was shared.

4. **Group**: Represents a group of users. Groups are used to manage permissions and facilitate the sharing of files with multiple users at once.
    - **Attributes**:
        - `id`: Unique identifier for each group (Primary Key).
        - `name`: Name of the group.
        - `description`: Description of the group.
        - `createdAt`: Timestamp indicating when the group was created.

5. **UserGroup**: Represents the many-to-many relationship between users and groups.
    - **Attributes**:
        - `id`: Unique identifier for each user-group association (Primary Key).
        - `user`: Reference to the user (Foreign Key referencing `User`).
        - `group`: Reference to the group (Foreign Key referencing `Group`).

### **1.2. Relationships**

- **User-File Relationship**: One user can own multiple files, establishing a one-to-many relationship.
- **User-SharedFile-File Relationship**: Files can be shared with multiple users, creating a many-to-many relationship between users and files, which is managed by the `SharedFile` entity.
- **User-Group Relationship**: A user can belong to multiple groups, and a group can have multiple users, establishing a many-to-many relationship managed by the `UserGroup` entity.
- **Group-SharedFile Relationship**: Files can be shared with groups, enabling efficient management of file sharing with multiple users.

## 2. Description of the Complex Query

The system includes a complex query that retrieves all files that belong to a specific user, as well as all files that have been shared with that user, including details about the owner of each file. This query joins the `User`, `File`, and `SharedFile` tables to extract the relevant information.

**In words**: The query returns all files owned by a given user along with all files shared with the user, including metadata about each file and the owner of the shared files. This involves joining the `File` and `SharedFile` tables with the `User` table to collect details such as file name, type, size, owner information, and sharing timestamp.

## 3. Description of the Complex Business Logic Operation

The system supports a complex business operation that handles the sharing of a file with another user. When a file is shared, the system verifies if the file owner permits sharing, ensures that the file is not already shared with the same user, and then creates a new entry in the `SharedFile` table.

**In words**: When a user chooses to share a file, the business logic first validates whether the requesting user is the file owner. Next, it checks if the file has already been shared with the intended user to prevent duplicate entries. If these conditions are met, the system proceeds to create a new `SharedFile` entry, storing details such as the file reference, the user it is being shared with, the level of permission (e.g., read-only), and the timestamp of sharing. This operation is managed as a transactional process to ensure data consistency in case of any failures during the operation.
