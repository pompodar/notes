    const [searchTerm, setSearchTerm] = useState('');


<input

                        type="text"

                        className="rounded-lg px-6"

                    placeholder="Search by name"

                    value={searchTerm}

                    onChange={(e) => setSearchTerm(e.target.value)}

                />


{users.length === 0 ? (

                <p>No users found with the given search term.</p>

            ) : (

                users

                    .filter((user) => user.name.toLowerCase().includes(searchTerm.toLowerCase()))

                    .map((user) => (

                        <div className="pt-12">

                            <div className="max-w-7xl mx-auto sm:px-6 lg:px-8">

                                <div className="flex bg-white overflow-hidden shadow-sm sm:rounded-lg">

                                    <div className="p-6 text-gray-900">

                                        {user.name} {auth.user.id == user.id ? " (me)" : null}

                                    </div>

                                    <div className="p-6 text-gray-900">{user.email}</div>

                                </div>

                            </div>

                        </div>

                    ))

