---
layout: post
title:  "creating an elasticsearch index with Python"
categories: Learning Python
image: /assets/elasticsearch.png
---

How to create and populate a new index on an already existing elasticsearch server.
<!--more-->


Elasticsearch databases are great for quick searches.  Let's imagine we already have a pandas dataframe ready, *data_for_es*, to pop into an index and be easily search.

### Connect to elasticsearch host
We can easily connect to our host using the elasticsearch library.

		# configure elasticsearch
		config = {
		    'host': 'XXX.XX.X.XXX'
		}
		es = elasticsearch.Elasticsearch([config,], timeout=300)

### Create new index
Choose the number of shards and replicas your index requires.  Elasticsearch divides the data into different shards.  Each shard is replicated across nodes.  

Mapping tells elasticsearch what kind of data each field contains.  *analyzed* or *not_analyzed* refers whether a string is analysed before it is indexed.  So a field that is *not_analyzed* will be mapped as an exact value.  Since both type of field get indexed, both are searchable.

		request_body = {
		    "settings" : {
		        "number_of_shards": 5,
		        "number_of_replicas": 1
		    },

		    'mappings': {
		        'examplecase': {
		            'properties': {
		                'address': {'index': 'not_analyzed', 'type': 'string'},
		                'date_of_birth': {'index': 'not_analyzed', 'format': 'dateOptionalTime', 'type': 'date'},
		                'some_PK': {'index': 'not_analyzed', 'type': 'string'},
		                'fave_colour': {'index': 'analyzed', 'type': 'string'},
		                'email_domain': {'index': 'not_analyzed', 'type': 'string'},
		            }}}
		}
		print("creating 'example_index' index..."
		es.indices.create(index = 'example_index', body = request_body)

### Prepare data
Each cell from the pandas dataframe must be input to the elasticsearch index with its metadata.

		bulk_data = []

		for index, row in data_for_es.iterrows():
		    data_dict = {}
		    for i in range(len(row)):
		        data_dict[data_for_es.columns[i]] = row[i]
		    op_dict = {
		        "index": {
		            "_index": 'example_index',
		            "_type": 'examplecase',
		            "_id": data_dict['some_PK']
		        }
		    }
		    bulk_data.append(op_dict)
		    bulk_data.append(data_dict)

### Input data
Finally, the data is ready to be input to the elasticsearch index.

		res = es.bulk(index = 'example_index', body = bulk_data)

		# check data is in there, and structure in there
		es.search(body={"query": {"match_all": {}}}, index = 'example_index')
		es_indices.get_mapping(index = 'example_index')


### Feedback
Always feel free to get in touch with other solutions, general thoughts or questions.
