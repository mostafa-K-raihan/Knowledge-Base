- Debugging nock
	- `DEBUG=nock.* yarn test`
- Get a single file from github
	- `curl -OL https://raw.githubusercontent.com/<username>/<repo-name>/<branch-name>/path/to/file`
- How to unzip a gz file
	- `gzip -d filename.gz` 
- How to import sql file into your db
	- `docker exec -i mysql_container mysql -uroot -psecret database < database_backup.sql`
	- Make sure your sql does not contain `Drop table` command, otherwise u will lose data
- Typescript: Cannot find namespace
	- In case you are a React dev that ends up here as I did - if there is JSX syntax in the file, try changing the extension of the file from .ts to .tsx.