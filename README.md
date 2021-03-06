
# lifeform

lifeform accepts a *life classification id* and fetches a portfolio of information. 

> Life classification ids are as assigned by ncbi.
>
> ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=**123**

### Getting Started
At its simplest.

[![NPM](https://nodei.co/npm/lifeform.png)](https://npmjs.org/package/lifeform)

```js
const lifeform = require('lifeform')
console.log(await lifeform.find('123'))

/*{ [Object lifeform]
    
    id: '123',
    article: [ 'lorum ipsum chunk', 'lorum ipsum chunk' ],
    description: 'lorum ipsum chunk',
    facts: { discoveryYear: 1998, },
    imageURL: 'imgur.com/b0bBy.png,
    lineage: [ 'id_of_kingdom', 'id_of_family', 'id_of_genus', ],
    links: [ { type: 'paper', title: 'Bacillus of the North', url: '', } ],
    name: 'bacterius bobberius',
    parentWithImage: 'id',
    rank: 'species',
    thumbnail: ';base64'
}*/
```

### Find

The only method is `lifeform.find(id, [include])`.

`[include]` is an array that allows you to specify a list of keys to return. The example in the *Getting Started* section above shows the fields which are returned by default (all of them).

```js
lifeform.find('123', ['name', 'description']).then(console.log)

/*{ [Object lifeform]

    id: '123',
    name: 'bacterius bobberius',
    description: 'lorum ipsum chunk'
}*/
```

### Images

There are three different image attributes.

If an image is found:

- `imageURL` provides the url of a matched image.
- `thumnail` processes that image into a small thumbnail encoded as base64.

If no image can be found, it will instead return the `parentWithImage` value. This is the id of the nearest parent that has an associated image. We walk up the tree of life until we find an ancestor with an image and then return this ancestor's id. 

> This is so that a parent image could be used as a 'visual group placeholder' for this type of life, which may be useful in some UX contexts.

### Notes

- Conservative. Better no match than an incorrect match.
- All of the functions that make API calls to external services need rate-limiters attached. Be careful, we're not cacheing anything and these services will block your IP in a heartbeat.