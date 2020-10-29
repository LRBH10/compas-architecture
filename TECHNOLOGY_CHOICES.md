# Technology Choices
The right technology choices are key to a good project.

# Databases
These databases have been evaluated:
- PostgreSQL
- Neo4J
- CouchDB
- eXist-db

## Checks for determining database
- Database should be able to store XML data
- Database should be relational, because we need relations between files/pieces of files/models
- Database must be provided with the solutions, for example part of a Docker image
- Support must be available, in case of trouble.
- Database must be open-source, because the whole solution must be open-source

## PostgreSQL
Homepage: https://www.postgresql.org/

### Pros
- Open Source (not entirely, working with patches)
- Fully supports ACID

### Cons
- Does not support XML out-of-the-box

## Neo4J

## CouchDB

## eXist-db
Homepage: http://exist-db.org/exist/apps/homepage/index.html

### Pros
- Native XML database
- Fully Open Source
- Cross-platform
- Active community (weekly community call, Slack channels, books)
- Docker image available
- Multiple query languages like HTTP, REST, xQuery and xPath
- Can act like a graph database, so relations between xml fragments for example is possible.
- Supports XML validation (https://exist-db.org/exist/apps/doc/validation.xml)

### Cons
- Using LGPL software is discouraged by Alliander when __modifying__ source code. So if we want to add features to eXist-db in the future, we might have a problem.
- No SQL available (is this needed?)
- Doesn't have all the ACID properties according to [vschart](http://vschart.com/compare/exist-db/vs/postgresql). Isolation is unknown, can't find other sources which confirm this.
- Not well known, no clear use cases in production. There are some [here](http://showcases.exist-db.org/exist/apps/Showcases/index.html)
- eXist needs JRE (Java Runtime Environment) to run

# XML Processing
## Checks for determining XML processing
- Can manipulate/check XML configuration files by using rules
- Can be embedded in our solution
- Tool must be open source

## RiseClipse
### Pros
- Main use is validating IEC 81850/IEC CIM configuration files, exactly what we need.
- Usage experience within Alliander
- [Docker image](https://hub.docker.com/r/riseclipse/riseclipse-validator-scl) available
- Add own validation rules (in Object Constraint Language)

### Cons
- Development doesn't seem very active
- Community is limited / not very active ( See [GitLab](https://gitlab-research.centralesupelec.fr/groups/RiseClipseGroup/-/activity) and [GitHub](https://github.com/riseclipse) )

## Schematron
### Pros
- There is a XQuery library module for eXist-db (https://github.com/Schematron/schematron-exist)
- Rule-based approach. If assertion fails, a message is being supplied
- Based on XSLT and xPath, so very flexible in manipulating/processing XML
- Suggesting XML fixes
- Referencing other XML documents as constraint validation
- XSL Processor like [Saxon-HE](http://saxon.sourceforge.net/) is easy to use

### Cons
- Not an application itself, needs a XSLT processor like [Saxon-HE](http://saxon.sourceforge.net/) which is also open-source

## Advice Rob
My advice would be to use Schematron (in combination with an XSLT processor) as the XML processing tool. It can do what RiseClipse can do, and more (like suggesting XML fixes and it's more flexible because it works with native XML technologies). Plus, it works in combination with eXist-db. 

Examples:
https://en.wikibooks.org/wiki/XQuery/Validation_with_Schematron#Setup_in_eXist-db

https://exist-db.org/exist/apps/doc/validation

https://github.com/Schematron/schematron-exist

RiseClipse is also a good candidate, because it's dedicated on IEC 61850/CIM validation. Only thing is, it's not as flexible as using Schematron. But I really do like the combination Schematron and eXist-db.