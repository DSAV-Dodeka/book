# Schema

## Migrations

We can use Alembic for migrations, which allow you to programatically apply large schema changes to your database.

First you need to have the Poetry environment running as described earlier and ensure the database is on as well. 

* Navigate to the `./backend/src/schema` directory.

### Ensuring the database is in sync with the migrations

The migrations are stored in the `schema/model/versions` folder. First, make sure your database has the same schema as the latest revision.


* From there run `poetry run alembic revision --autogenerate -m "Some message"`
* This will generate a Python file in the migrations/versions directory, which you can view to see if everything looks good. It basically looks at the database, looks at the schema described in db/model.py and generates code to migrate to the described schema.
* Then, you can run `poetry run alembic upgrade head`, which will apply the latest generated revision. If you now use your database viewer, the table will have hopefully appeared.
* If there is a mismatch with the current revision, use `poetry run alembic stamp head` before the above 2 commmands.