---
id: AsyncPG Connector
aliases:
  - Example asyncpg connector
tags:
  - python
  - asyncpg
---
# Example asyncpg connector

```python
  import asyncpg
  import asyncio

  class PostgresConnector:
      _instance = None

      def __new__(cls):
          if cls._instance is None:
              cls._instance = super().__new__(cls)
              cls._instance.connection_pool = None
          return cls._instance

      def __init__(self, database_uri, max_connections=10):
          self.database_uri = database_uri
          self.max_connections = max_connections
          self.loop = asyncio.get_event_loop()
          self.loop.run_until_complete(self.initialize())

      async def initialize(self):
          self.connection_pool = await asyncpg.create_pool(self.database_uri, max_size=self.max_connections)

      async def execute(self, query, *args):
          async with self.connection_pool.acquire() as connection:
              return await connection.execute(query, *args)

      async def fetch(self, query, *args):
          async with self.connection_pool.acquire() as connection:
              return await connection.fetch(query, *args)

      async def close(self):
          await self.connection_pool.close()

  async def main():
      database_uri = "postgresql://username:password@localhost/dbname"
      max_connections = 5

      connector = PostgresConnector(database_uri, max_connections)

      # Example usage
      query = "SELECT * FROM users WHERE age > $1"
      result = await connector.fetch(query, 18)
      print(result)

      await connector.close()

  if __name__ == "__main__":
      asyncio.run(main())
```
