# Applications and Middleware Modernization
This repo provides narrative, architecture guidance and reference implementation for modernization of enterprise Applications and Middleware, such as IBM WebSphere Application Server, or other JEE App servers, IBM MQ, IBM Integration Bus and others, running today on-premises inside the walls of traditional datacenters.
**Jump to middleware type specific subdomains:**
- [Traditional IBM WAS and JEE Applications Modernization](jee-mod)
- [IBM MQ Modernization](mq-mod)
- [IBM Integration Estate Modernization](integration-mod)
- [Additional Resources](#additional-resources)

## Modernization Paths
The approaches to Enterprise Applications and Middleware modernization can be roughly divided into 3 main areas:
[Modernization with Cloud Platform]() 
[SaaS Adoption]()
[In-place modernization]()
The decisions which path to pursue is made based on Modernization Assessment, assisted by IBM Transformation Advisor (link)
![Modernization Paths](static/imgs/modernization-paths.png?raw=true)

### Modernization based on Cloud Platform
First direction to modernization is much aligned with transformation based on a cloud platform associated with workload containerization.  There are 3 distinct paths in this space.  
Refactoring is essentially a rewrite of the enterprise application following cloud native principles such as microservices with primary objective of accelerating innovation.  This is greenfield area as new code is built from the ground up.  
Repackaging refers to more evolutionary modernization approach sometimes referred to as brownfield where existing code assets are repackaged to be able to run in the cloud based container runtimes.  The repackaged applications are not just made cloud ready from runtime compatibility perspective, but they are substantially enhanced from functionality perspective, e.g. offer entirely different UX, but retain monolithic logic implementation. 
Replatforming approach is akin to “lift and shift” strategy as it is defined by minimizing change required.  This may be not the most appealing path from purely application modernization perspective as there is no new business function, but this is approach is very interesting from pure middleware modernization angle. In fact, containerized middleware is the next generation in middleware evolution and replatforming middleware on container based platform is essentially equivalent to upgrading to next gen middleware wave.

### SaaS Adoption
This direction leads to replacing select enterprise applications with corresponding SaaS offerings.  The key challenge in such modernization path is a need to integrate new SaaS offerings into existing enterprise and cloud landscape.  This creates dependency of SaaS modernization path on modern Agile Integration platform, which is provided by Cloud platform mentioned earlier.

### In place modernization
Some enterprise assets can’t go to cloud and need to be modernized in place staying on prem.  2 main aspects of in-place modernization is version currency and API enablement. Version currency is not only dictated by obvious need to stay on supported levels of software, be able to receive security upgrades and leverage ongoing improvements in QoS.  Very important aspect of upgrading enterprise software to current versions is achieving “cloud ready” state. This means that current versions of enterprise software are much more compatible with cloud environments, and enable transition to cloud much easier in the future, if the decision to stay on premise was primarily dictated by cost and complexity of the migration.