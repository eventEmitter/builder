# TBD


Builds standardized object trees (query-tree) for loading relational data 
from a datasource. Attempts to make your live easier by providing a simple 
and clean interface for building query trees that then can be sent to a 
compatible data source.



    const event = new builder.Event({
        name: 'fest',
        venue: builder.venue({
            nme: 'joinbox'
        })
    });

    await event.save();




    const data = await builder.getEvent('*', {
        id: 5
    }, builder.filter.or().getVenue({
        id: 5
    }).up().fetchImage({
        width: builder.F.LT(5)
    })).getVenue('*').image('*').finOne();




    {
        type: 'event',
        properties: [{
            name: 'id',
            isId: true,
            isUnique: true,
            references: ['venue'],
            referencedBy: ['image'],
            type: 'string',
            constraints: [{
                type: 'notMatch',
                value: '/[^0-9]/gi',
            }, {
                type: 'max'
                value: '4294967296'
            }, {
                type: 'min'
                value: '0'
            }],
            defaultValue: '',
            writeable: true,
            readable: true,
            nullable: true,
            restrictions: [{
                property: 'event.category.identifier',
                value:['party'],
                comparator: 'oneOf',
            }],
            created: date,
            deprecated: date,
            removed: date,
            description: 'contains the unique id for the record',
        }],
        actions: [{
            name: 'create',
            allowed: true,
            bulk: true,
            responses: ['ok', 'seeOther']
        }],
        relations: [{
            name: 'venue',
            type: 'reference',
            writeable: true,
            readable: true,
        }],
        responses: [{
            class: 'success'
            code: 'ok'
            description: 'everything went well'
            data: []
        }, {
            class: 'success'
            code: 'processing'
            description: 'everything went well'
            data: []
        }, {
            class: 'redirect'
            code: 'seeOther'
            description: 'the resource can be found at another address'
            method: 'list'
            filter: {
                type: 'property'
                value: 'id'
                children: [{
                    type: 'comparator'
                    value: 'equal'
                    children: [{
                        type: 'value'
                        value: 6
                    }]
                }]
            }
            organization: 'joinbox'
            application: 'timers'
            service: 'user'
            resource: 'userLoginEmail'
        }, {
            class: 'userError|clientError|serverError|success|redirect'
            code: 'validation_error'
            severity: 'error|warning|info'
            description: 'the validation for the field ${x} has failed!'
            retry: false
        }]
    }


response codes and their response format must bre registered at the central database


    {
        type: 'request',
        action: 'create',
        locales: [{
            language: 'de'
            country: 'ch'
        }]
        data: {

        }
    }


    {
        type: 'request'
        action: 'list'
        locales: [{
            language: 'de'
            country: 'ch'
        }]
    }


    {
        type: 'response'
        code: 'ok'
        class: 'success'
        data: [{
            id: 2
            floor: {
                type: 'response'
                code: 'ok'
                class: 'success'
                data: [{
                    id: 2
                    floor: 
                }]
            }
        }]
    }


    {
        type: 'response'
        class: 'clientError'
        code: 'contraint_error'
        property: 'name'
        failedContraints: [{
            type: 'notMatch',
            value: '/[^0-9]/gi',
            description: 'Expected numbers only!'
        }, {
            type: 'max'
            value: '4294967296'
            description: 'Expected a 32 bit unsigned integer, which is a number between 0 and 4294967296'
        }],

    }



    {
        type: 'response'
        class: 'success'
        code: 'created'
        data: {}
    }




- client: consumes the data, client facîng business logic
------
- server: routes incoming messsage, does the routing
------
- frontend: a html generating frontend for sending da
------
- facade: combines resources, is only used for returning combines data
------
- controller: contains business logic, 
------
- model: represenation of the data structure, vlaidation, model definition
------
- dataSource: the source for the data, the abstaction of dbs, files and apis