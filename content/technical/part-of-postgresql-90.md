Title: Part of Postgresql 9.0...
Date: 2010-05-07
Tags: technical
Slug: part-of-postgresql-90

I've
contributed to another open source project, Postgresql. My first
contribution [made it into version 9.0][].</span>  

I
worked on the ```samenet``` and
```samehost```
host
based access control feature, which lets you grant database access to
hosts on the physical subnets that the postgresql server is attached
to.

Previously many postgresql
deployments for clients used to have
```0.0.0.0/0```
in
the
pg_hba.conf
file,
because more limited access controls were too brittle and would
inevitably fall over when the client renumbered their network.

  [made it into version 9.0]: http://developer.postgresql.org/pgdocs/postgres/release-9-0.html
