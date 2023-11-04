
 // Pagination state

    const [currentPage, setCurrentPage] = useState(1);

    const usersPerPage = 1; // Set the number of users to display per page

// Calculate Pagination Parameters

    const indexOfLastUser = currentPage * usersPerPage;

    const indexOfFirstUser = indexOfLastUser - usersPerPage;

    const currentUsers = usersList.slice(indexOfFirstUser, indexOfLastUser);

  

    // Handle Pagination Controls

    const totalPages = Math.ceil(usersList.length / usersPerPage);

  

    const nextPage = () => {

        if (currentPage < totalPages) {

            setCurrentPage(currentPage + 1);

        }

    };

  

    const prevPage = () => {

        if (currentPage > 1) {

            setCurrentPage(currentPage - 1);

        }

    };

<div className="ml-8 mt-2 w-40 flex justify-between items-center">

                    <button onClick={prevPage} disabled={currentPage === 1}>previous</button>

                    <button onClick={nextPage} disabled={currentPage === totalPages}>next</button>

                </div>