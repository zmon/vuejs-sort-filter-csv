<template>
    <div>
        <h1>Sort and Filter</h1>
        <label>
            Search:
            <input v-model="filterBy">
        </label>
        <table>
            <tr>
                <th >id</th>
                <th v-on:click="sortBy('first_name')">First Name</th>
                <th v-on:click="sortBy('last_name')">Last Name</th>
            </tr>
            <tr  v-for="user in this.usersSorted" :key="user.id">
                <td>{{ user.id }}</td>
                <td>{{ user.first_name }}</td>
                <td>{{ user.last_name }}</td>

            </tr>
        </table>

    </div>
</template>

<script>
    import _ from 'lodash';  // Allows us to do _.orderby in usersSorted()
    export default {
        data: function () {
            return {
                users: [    // We can use https://www.papaparse.com/ to load a CSV
                    {
                        "id": 4,
                        "first_name": "Zues",
                        "last_name": "Holt"
                    },
                    {
                        "id": 5,
                        "first_name": "Charles",
                        "last_name": "Morris"
                    },
                    {
                        "id": 6,
                        "first_name": "Abbuy",
                        "last_name": "Ramos"
                    }
                ],
                sortKey: ['customer_name'],
                sortOrder: ['asc'],
                filterBy: '',
            }
        },
        computed: {
            usersSorted: function() {
                return _.orderBy(this.users, this.sortKey, this.sortOrder)
                    .filter(user => {
                        if (this.filterBy) {
                            var searchFor = this.filterBy.toLowerCase();        // lets change everthing to lower case

                            // indexOf returns -1 if not found else
                            return ((user.first_name.toLowerCase().indexOf(searchFor)) !== -1)
                            || ((user.last_name.toLowerCase().indexOf(searchFor)) !== -1);

                        } else {
                            return true
                        }
                    });
            },
        },
        methods: {
            sortBy: function (key) {

                if (key == this.sortKey) {
                    this.sortOrder = (this.sortOrder == 'asc') ? 'desc' : 'asc';
                } else {
                    this.sortKey = key;
                    this.sortOrder = 'asc';
                }
            },
        }
    }
</script>
