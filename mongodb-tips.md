# Map-Reduce example - group documents by reg-exp match of a particular field value

The goal of this map-reduce command is to group documents by first alpha-numeric character of the `tradeMark` field.

```js
db.runCommand({
    mapReduce: "business", // collection name
    map: function () {
        // get the first letter or digit of the tradeMark field
        var key = this.tradeMark ? (this.tradeMark.match(/[а-яa-z0-9]/i) || ['']).pop() : '';
        // emit key-value pair, uppercase the key
        emit(key.toUpperCase(), this);
    },
    reduce: function (key, values) {
        // if multiple values exists for the key, then this function will be called
        var result = {key: key, value: []};
        // ignore non array values
        if (values && Array.isArray(values)) {
            values.forEach(function (v) {
                // ignore falsy values
                if (v) {
                    result.value.push(v);
                }
            });
        }
        return result;
    },
    finalize: function (key, reducedValue) {
        // normalize results
        var result = [];

        function process(item) {
            // if item doesn't have key-value fields, then assume it as plain document
            if (!item.key && !item.value) {
                result.push(item);
                return;
            }
            // if item has array type value field, then process each item individually
            if (item.value && Array.isArray(item.value)) {
                item.value.forEach(process);
                return;
            }
        }

        // process the value
        process(reducedValue);

        // just return populated results, no need to return key, cause each item has _id field that contains key
        return result;
    },
    out: {"inline": 1},
    query: {},
    sort: {},
    inputDB: "paycom",
});
```

Sample output:
```
{
	"results" : [
		{
			"_id" : "",
			"value" : [
				{
					"tradeMark" : null,
					"active" : false,
					"name" : "Рога и Копыта"
				}
			]
		},
		{
			"_id" : "0",
			"value" : [
				{
					"tradeMark" : "00100",
					"active" : false,
					"name" : "MUSTAFOKULOV MUKIMJON"
				}
			]
		},
		{
			"_id" : "A",
			"value" : [
				{
					"tradeMark" : "AI",
					"active" : false,
					"name" : "Ani Ani"
				},
				{
					"tradeMark" : "Asiatour",
					"active" : false,
					"name" : "Asia Tour Elite"
				}
			]
		},
		{
			"_id" : "B",
			"value" : [
				{
					"tradeMark" : "Bek Catering",
					"active" : true,
					"name" : "Al Boss-Servis"
				},
				{
					"tradeMark" : "Burritos",
					"active" : true,
					"name" : "Burritos Servis"
				}
			]
		}
		// ...
	],
	"timeMillis" : 36,
	"counts" : {
		"input" : 1086,
		"emit" : 1086,
		"reduce" : 201,
		"output" : 57
	},
	"ok" : 1
}
```