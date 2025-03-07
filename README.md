# Hasura table/column/relations camelizer

## CLI

```bash
npm i -g hasura-camelize

# Easy
hasura-camelize --host https://some.domain --secret some-secret

# Dry
hasura-camelize --host https://some.domain --secret some-secret --dry

# Exclude table names
hasura-camelize --host https://some.domain --secret some-secret --exclude some_table

# Include table names
hasura-camelize --host https://some.domain --secret some-secret --include some_table

# Rename relations
hasura-camelize --host https://some.domain --secret some-secret --relations
```

## From code

### Simple

```ts
import convert from 'hasura-camelize';

convert(
  // connection settings
  {
    // domain name
    host: 'https://some.domain',
    // or ip
    host: 'http://127.0.0.1:3000',
    // admin secret
    secret: 'some-secret',
  }
);
```

### Complex

```ts
import convert from 'hasura-camelize';

convert(
  // connection settings
  {
    // domain name
    host: 'https://some.domain',
    // or ip
    host: 'http://127.0.0.1:3000',
    // admin secret
    secret: 'some-secret',
    // optional schema (default 'public')
    schema: 'public',
    // optional source (default 'default')
    source: 'default',
  },
  // optional settings
  {
    // Dry run? (don't apply changes)
    dry: false,
    // Rename relations? (default false)
    relations: true,
    // Transform table names differently
    transformTableNames(
      name,
      defaultTransformer
    ) {
      // if name === some_name then ignore the table
      if (name === 'some_name') return false;
      return defaultTransformer(name);
    };
    // Transform column names differently
    transformColumnNames(
      name,
      tableName,
      defaultTransformer
    ) {
      // if name === some_name then ignore the column
      if (name === 'some_name' && tableName === 'some_name') return false;
      return defaultTransformer(name);
    };
    // Apply different root field names
    getRootFieldNames(
      name,
      defaultTransformer
    ) {
      return defaultTransformer(name);
    };
  }
);
```

## Credits

- Original github issue in hasura repository: https://github.com/hasura/graphql-engine/issues/3320
- User @svarlamov for https://github.com/exlinc/hasura-enforce-camel-case
- User @m-rgba for https://github.com/m-rgba/hasura-snake-to-camel
