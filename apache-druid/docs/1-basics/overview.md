## O V E R V I E W

### About this part

The Apache Druid database powers analytical applications for organizations large and small [around the world](https://druid.apache.org/druid-powered). [Under the hood](https://druid.apache.org/docs/latest/design/), a highly-optimized data format and shared-nothing, microservices architecture helps deliver on performance, resilience, and availability. On the surface, a fully-fledged SQL dialect enables you to create flexible, interactive analytics UI components for your applications using real-time and batch ingested data.

### Outcome

- Create a single node and clustered Druid database in a virtual lab
- Hear about Druid's processes and how they come together in a shared-nothing, microservices architecture
- Ingest a simple dataset and get a feel for how this works
- See the different SQL interfaces for querying Druid

### What do we need?

Familiarity with Linux is a bonus, but by no means a prerequisite.

## C U R R I C U L U M

<details open>

<summary>TRY DRUID</summary>

<hr/>

📄 [Introduction](./1.1-try-it-out/introduction.md)

⤷ [Set up a small Druid server](./1.1-try-it-out/1-installation.md)

⤷ [Use some example data to create a Druid table](./1.1-try-it-out/2-ingestion.md)

⤷ [Retrieve the example data using SQL](./1.1-try-it-out/3-querying.md)

📄 [Learn More](./1.1-try-it-out/learn-more.md)

</details>

<details open>

<summary>DRUID ARCHITECTURE</summary>

<hr/>

📄 [Introduction](./1.2-architecture/introduction.md)

⤷ [Understand how Druid organizes its servers and processes](./1.2-architecture/1-servers-and-proceses.md)

⤷ [Stand up a multi-node Druid cluster](./1.2-architecture/2-deployment.md)

⤷ [Use the same data from the previous module to create a Druid table](./1.1-try-it-out/2-ingestion.md)

⤷ [Perform the same queries as before, but this time on a full cluster](./1.1-try-it-out/3-querying.md)

📄 [Learn More](./1.2-architecture/learn-more.md)

</details>
