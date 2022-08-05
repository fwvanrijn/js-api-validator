<template>
    <div>
        <div>
            <button @click="getApiData('bars')">Get Bars</button>
            <button @click="getApiData('cars')">Get Cars</button>
            <button @click="getApiData('persons')">Get Persons</button>
        </div>
        <div v-for="error in errors" :key="error"><span v-html="error.msg"></span></div>
    </div>
</template>

<script>
export default {
    name: "ApiValidator",

    data () {
        return {
            validationSchema: {
                bars: {
                    name: 'string',
                    address: 'string',
                    drinks: 'object',
                },
                cars: {
                    brand: 'string',
                    type: 'string',
                    milage: 'number',
                    extras: 'array',
                },
                persons: {
                    name: 'string',
                    age: 'number',
                    siblings: 'array',
                    metaData: 'object',
                    active: 'boolean',
                },
            },
            bars: [
                {
                    name: 'Jimmys drinks',
                    address: 'Somewhere over the rainbow',
                    drinks: {
                        beer: ['Straffe Hendrik', 'Rochefort', 'St Bernard'],
                    },
                },
                {
                    name: 'Sjonnies',
                    address: 'Centrum 001',
                    drinks: [ // < No object
                        'Heineken',
                    ]
                }
            ],
            cars: [
                {
                    brand: 'Mazda',
                    type: 'MX5 NB 1.8',
                    milage: 199999.99,
                    extras: ['2001 Special Edition'],
                },
                {
                    brand: 'BMW',
                    type: '335',
                    milage: '100000', // < No number
                    extras: [
                        'LCI',
                        'KW Coilovers',
                    ],
                }
            ],
            persons: [
                {
                    name: 'James',
                    age: 25,
                    siblings: ['Johnnathan'],
                    metaData: {},
                    active: true,
                },
                {
                    name: 'James',
                    age: 25,
                    active: true,
                }
            ],
            errors: [],
        }
    },

    methods: {
        /**
         * Get data
         *
         * @param {string} type
         */
        getApiData(type) {
            this.fakeApiCall(type).then(response => {
                response.forEach((responseObject) => {
                    this.validate(type, responseObject);
                });
            });
        },

        /**
         * Check if properties of the response Object match the type of the validation scheme.
         * Note that typeof array is object.
         *
         * @param {string} type
         * @param {Object} responseObject
         */
        validate(type, responseObject) {
            let validationSchema = this.validationSchema[type];
            let properties = Object.keys(validationSchema);

            properties.forEach((property) => {
                if (typeof responseObject[property] !== validationSchema[property] &&
                    ! (validationSchema[property] === 'array' && Array.isArray(responseObject[property]))) {
                    this.errors.push({
                        msg: 'error getting ' + type + ': got ' + typeof responseObject[property] + ' for property ' + property + ' expected ' + validationSchema[property],
                        data: responseObject,
                    });
                }

                if (validationSchema[property] === 'object' && Array.isArray(responseObject[property])) {
                    this.errors.push({
                        msg: 'error getting ' +type + ': got array for property ' + property + ' expected object',
                        data: responseObject,
                    });
                }
            });
        },

        /**
         * Fake an api call
         * @param {string} type
         * @returns {Promise<unknown>}
         */
        fakeApiCall(type) {
            return new Promise((resolve) => {
                resolve(Object.values(this[type]));
            });
        },
    }
}
</script>

<style scoped>

</style>