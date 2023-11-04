const [sortCriterion, setSortCriterion] = useState('name'); // Default sorting by name const [sortOrder, setSortOrder] = useState('asc'); // Default sorting order

<button onClick={() => setSortCriterion('name')}>Sort by Name</button> <button onClick={() => setSortCriterion('email')}>Sort by Email</button> <button onClick={() => setSortOrder(sortOrder === 'asc' ? 'desc' : 'asc')}> {sortOrder === 'asc' ? 'Ascending' : 'Descending'} </button>

{users.length === 0 ? ( <p>No users found with the given search term.</p> ) : ( users .filter((user) => user.name.toLowerCase().includes(searchTerm.toLowerCase())) .sort((a, b) => { if (sortCriterion === 'name') { return sortOrder === 'asc' ? a.name.localeCompare(b.name) : b.name.localeCompare(a.name); } else if (sortCriterion === 'email') { return sortOrder === 'asc' ? a.email.localeCompare(b.email) : b.email.localeCompare(a.email); } return 0; // No sorting }) .map((user) => (
