# Decentralized Social Login Smart Contract

## Overview
The **Decentralized Social Login Smart Contract** allows users to register, update, and manage their profile information on a blockchain network. The contract ensures that usernames are unique and email addresses follow valid formatting rules. Users can also set, clear, or delete their profiles securely.

## Features
- **User Registration**: Allows users to create an account with a unique username and valid email.
- **Profile Management**: Users can update their username, email, and profile image.
- **Username Availability Check**: Ensures that usernames are unique before registration.
- **User Deletion**: Users can delete their profiles, freeing up usernames for reuse.
- **Security & Validation**: Ensures data integrity by validating username, email, and image URL formats.
- **Decentralized Storage**: Stores user information securely on-chain.

## Smart Contract Functions

### Error Messages
The contract defines several error messages to maintain data integrity:
- `ERR-USER-EXISTS`: User already exists.
- `ERR-USER-NOT-FOUND`: User not found.
- `ERR-INVALID-USERNAME`: Username must be between 3 and 50 characters.
- `ERR-INVALID-EMAIL`: Email must be between 5 and 100 characters and contain '@' and '.'.
- `ERR-INVALID-IMAGE-URL`: Invalid profile image URL.
- `ERR-USERNAME-TAKEN`: The chosen username is already in use.

### Data Structures
- **Users Map (`users`)**: Stores user profiles, indexed by `principal`.
  ```clojure
  (define-map users principal
    {
      username: (string-ascii 50),
      email: (string-ascii 100),
      profile-image: (optional (string-utf8 256))
    }
  )
  ```
- **Taken Usernames (`taken-usernames`)**: Ensures usernames are unique.
- **User Count (`user-count`)**: Stores the total number of registered users.

### Public Functions

#### `register-user (username (string-ascii 50), email (string-ascii 100)) → (ok true | err message)`
Registers a new user if the username is available and the email is valid.

#### `update-profile (new-username (string-ascii 50), new-email (string-ascii 100)) → (ok true | err message)`
Updates a user’s profile details, ensuring the new username is available.

#### `delete-profile () → (ok true | err message)`
Deletes the user profile and removes the username from the taken list.

#### `set-profile-image (image-url (string-utf8 256)) → (ok true | err message)`
Updates a user’s profile image with a valid URL.

#### `clear-profile-image () → (ok true | err message)`
Clears the user's profile image.

#### `get-user-info (user principal) → (option {username, email, profile-image})`
Retrieves stored user information.

#### `get-user-count () → (ok uint)`
Returns the total number of registered users.

#### `is-user-registered (user principal) → (ok bool)`
Checks if a user is registered.

#### `is-username-available (username (string-ascii 50)) → (ok bool | err message)`
Checks if a username is available for registration.

## Validation Rules
- **Username** must be between 3 and 50 characters.
- **Email** must be between 5 and 100 characters and contain both '@' and '.'.
- **Profile Image URL** must be a valid UTF-8 string (up to 256 characters).

## Security Considerations
- Ensures unique usernames via the `taken-usernames` map.
- Prevents duplicate registrations using `users` map checks.
- Only the user (via `tx-sender`) can modify or delete their profile.
- Ensures that only valid email and username formats are stored.

## License
This contract is open-source and free to use under the MIT License.

---
This README provides an in-depth guide to using the **Decentralized Social Login Smart Contract**. If you have any questions, feel free to reach out!

