function deleteUser(userId) {

        if (window.confirm('Are you sure you want to delete this user?')) {

            Axios.defaults.xsrfCookieName = 'XSRF-TOKEN'; // The name of the CSRF cookie in Laravel

            Axios.defaults.xsrfHeaderName = 'X-XSRF-TOKEN'; // The name of the CSRF token header in Laravel

  

            Axios.delete(`/users/${userId}`)

                .then((response) => {

                    // Update the user list by filtering out the deleted user

                    setUsersList((prevUsers) => prevUsers.filter((user) => user.id !== userId));

  

                    alert('User deleted successfully.');

                })

                .catch((error) => {

                    alert('Error deleting user. Please try again.');

                    console.log(error);

                });

        }

    }