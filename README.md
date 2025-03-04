# D1-Orm

✨ A simple, strictly typed ORM, to assist you in using [Cloudflare's D1 product](https://blog.cloudflare.com/introducing-d1/)

API reference can be found at https://d1-orm.pages.dev/modules

Docs can be found at https://d1-orm.pages.dev/guides

## Installation

This package can be [found on NPM](https://npmjs.com/package/d1-orm)

```sh
npm install d1-orm
```

## Usage

This package is recommended to be used with [@cloudflare/workers-types](https://github.com/cloudflare/workers-types) 3.16.0+.

```ts
import { D1Orm, DataTypes, Model } from "d1-orm";
import type { Infer } from "d1-orm";

export interface Env {
	// from @cloudflare/workers-types
	DB: D1Database;
}

export default {
	async fetch(request: Request, env: Env) {
		const orm = new D1Orm(env.DB);
		const users = new Model(
			{
				D1Orm: orm,
				tableName: "users",
			},
			{
				id: {
					type: DataTypes.INTEGER,
					primaryKey: true,
					autoIncrement: true,
					notNull: true,
				},
				name: {
					type: DataTypes.STRING,
					notNull: true,
					defaultValue: "John Doe",
				},
				email: {
					type: DataTypes.STRING,
					unique: true,
				},
			}
		);
		type User = Infer<typeof users>;

		await users.First({
			where: {
				id: 1,
			},
		});
		// Promise<User | null>
	},
};
```

For more information, refer to the [docs](https://d1-orm.pages.dev/guides).
