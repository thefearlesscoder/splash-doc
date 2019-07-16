# Overview
The search component uses ElasticSearch as a backend server for indexing documents. ElasticSearch uses a tool called Monstache to sync with MongoDB. A Vue component is used as the frontend for this search tool. This Vue component sends axios requests to ElasticSearch's RESTful API to receive search requests. A variety of search functions have been implemented such as suggest-as-you-type, fuzzy suggest-as-you-type, term searching and fuzzy searching.

# Components

## ElasticSearch Backend
This backend contains an index under which the experiment JSON files are stored. Different mappings are made under certain fields in these JSON documents. All textual fields are automatically mapped as text and keywords by ElasticSearch. However, to enable suggest-as-you-type functionality on certain fields these fields are given an additional `completion` mapping with the `fields` parameter. This additional *multi-field* has been named "autocomplete." More information can be found at https://www.elastic.co/guide/en/elasticsearch/reference/current/search-suggesters-completion.html

## Search Bar 
This component has two functionalities. One is the ability to suggest to users results as they type and the second is to navigate to the search page with the user's request in a URL query. It sends a specially configured *suggesting* query in a request at each keystroke to ElasticSearch. ElasticSearch will return the possible matches which get displayed in the menu. If no results turn up in the first request then a second request is sent, however this time with fuzzy results enabled. 

When the user clicks search or hits the enter key the appropriate input is inserted into a URL query and the user navigates to that URL which brings up the "Search Page Component."

## Search Page
This component will access the `search` URL query parameter and send a `match` query to ElasticSearch through axios. If there are no results then it will send a second request with fuzzy results enabled. This query has been configured to require that 90% of the terms in the query must be present in the searched field if there are more than 3 terms. Otherwise 100% of the terms must be in the searched field.

This component then displays the results and paginates them using the `page` parameter in the URL query. A maximum of 10 results are displayed per page. The component sends a request to ElasticSearch with the `from` parameter which specifies the starting result ElasticSearch displays. By default ElasticSearch only displays 10 results per query.


# Testing
The testing framework that we use is Jest plugin for Vue found here https://www.npmjs.com/package/@vue/cli-plugin-unit-jest. To run the tests execute `npm run test:unit`. You may find documentation on how to write the tests here, https://vue-test-utils.vuejs.org/. 

All of the tests may be found in the `tests` directory. Each testing file should test _one_ single file component. Though not necessary, in order to make it easier to match tests with their respective code, the tests directory structure should mirror that of the `src` directory. same folder as that of the component it is testing. The tests are named according to this schema {SingleFileComponentName}.spec.js For example if one were writing a test `Foo.vue` the corresponding name should be `Foo.spec.js`

For testing async behavior this page https://vue-test-utils.vuejs.org/guides/testing-async-components.html explains it quite well. Preferably use the `await flushPromises()` function rather than the `$nextTick()` to avoid nesting callbacks. `await flushPromises()` will wait for all Promises to resolve whereas `$nextTick()` will only wait for the top level promises to resolve and not any of its nested promises potentially creating problems.

