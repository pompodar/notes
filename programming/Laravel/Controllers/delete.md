
public function deleteUser($userId)

    {

        // Find the user by ID and delete it

        $user = User::find($userId);

  

        if (!$user) {

            return response()->json(['message' => 'User not found'], 404);

        }

  

        $user->delete();

  

        return response()->json(['message' => 'User deleted successfully']);

    }