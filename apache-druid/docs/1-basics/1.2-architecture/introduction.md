## DRUID ARCHITECTURE

The foundation for understanding real Druid deployments

- [**Servers and Processes**](1-servers-and-proceses.md) - Understand how Druid organizes its servers and processes
- [**Deployment**](2-deployment.md) - Stand up a multi-node Druid cluster
- [**Ingestion**](../1.1-try-it-out/2-ingestion.md) - Use the same data from the previous module to create a Druid table
- [**Query**](../1.1-try-it-out/3-querying.md) - Perform the same queries as before, but this time on a full cluster
- [**And more**](learn-more.md) - Investigate related material such as docs, white papers and articles

Why understand Apache Druid architecture?

- To know how to scale
- To diagnose problems

**Log files** for processes can be found here:

```ruby
tail /root/apache-druid-25.0.0/log/*.log
```
