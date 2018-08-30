# IBM MQ Modernization Overview
Many MQ users are trying to understand how they can adopt containers and what benefits they can deliver in an organization. Here we will consider the benefits and overall strategy to this journey towards containers.
Containerization provides significant benefits for an MQ estate, however is most valuable when it is approached as part of a wider containerization strategy.

## Benefits of a MQ container strategy
![Benefits of a MQ container strategy](static/imgs/mq-containers-benefits.png?raw=true)

Regardless of the software technology installed and configured within a container, there are a number of common benefits:

1.   **Infrastructure Optimization** – Isolation between running artefacts is often desirable, however previous virtualization technology has often imposed a large system resource cost. Containers provide better  **density**  within the infrastructure, when compared to virtual machines. This is due to containers having less memory overhead per instance, as each container reuses the existing host operating system. This allows us to tackle resource management issues using a new approach. For instance an existing multiple component solution, which was hosted on a single machine can be separated into multiple containers and each container can have its resources capped.

1.   **Scalability &amp; Availability** – Container orchestration technology provides the capability to scale containers horizontally, and balance the inbound traffic across the available instances.

1.   **Operational Consistency** - All parts of the solution are deployed and managed in the same way, as containers, regardless of the underlying technology. This provides a common deployment, management, operational and monitoring approach across the deployed software.

1.   **Team Agility**  – Often a container will be scoped to an individual team, allowing them to build their own solutions independently, react to change more rapidly, and reaching a greater team velocity. As containers are specific to a team and a particular project, they are smaller in nature, which allows for fast deployment.

1.   **Fine Grained Resilience** - Embrace the &quot;cattle instead of a pet&quot; approach so that if a container stops working, kill it off, and start a new one. This reduces the cost, effort of maintenance, and potentially the overall resilience of the solution. This simplifies the process of upgrading the solution, as the container will not hold any required state, the old versions can be disposed of, and the new versioned containers started.

1.   **Component Portability**  - Any platform with a container engine can run containers. This lowers the barrier to moving cloud providers.

A key benefit of using containers is that you get a more common operational model across multiple technologies. Therefore the adoption of containers should be part of an organization wide strategy instead of focusing on an individual technology, to truly realize the benefits.

## MQ Containerization Journey

Similar to the adoption of many other technologies, there will be a journey for the adoption of containers. 
The adoption of containers in the context of MQ can be separated into three stages or aspects, each providing independent and distinct set of benefits associated with containers. 

![MQ Containerization Journey Aspects](static/imgs/mq-journey-aspects.png?raw=true)

**Migrate to containers (Foundational)**: Many organizations have a traditional software on-premise deployment, and are keen to cloud enable. For some organizations this may mean moving to a public cloud provider, while others will take a more gradual approach containerizing their on-premise deployment. Once running within containers, this will facilitate  **component portability** , as containers can be copied to any platform that supports containers. The infrastructure team will streamline their operations across software products, as all software can be handled in a standard approach, providing  **operational consistency**. As the footprint of new containers is low, this increases the number of running containers a machine can handle, compared to virtual machines, providing the ** infrastructure optimization**. Finally the container orchestration technology will assure the containers are available, which provides a level of high availability and  **fine grained resilience**.

Although migrating to containers requires as little change as possible to MQ configuration and can be seen as a lift and shift, moving to a container strategy is likely to involve some changes in the messaging topology, such as removing local MQ connections and  introducing a network communication link between the applications and MQ processes.

**Decentralize for decoupling** : Application development is changing, from monolithic applications, to componentized applications. This allows the development of the components to be decoupled, and provides greater agility. It also introduces a new requirement for decoupled communication between components. It is often desirable to move the application messaging to within the application, away from the central messaging backbone.

New development architectures are being embraced within organizations, which facilitate greater  **team agility** , by decoupling dependencies between teams. To succeed, this needs to include resources such as IBM MQ. Within the organization there is normally an IBM MQ Administration team that owns and maintains the messaging platform, however individual development teams are eager for more control and access. To provide the additional flexibility, individual IBM MQ instances can be provided to the development teams, so they can be modified independently of the wider IBM MQ estate. Although the development teams will have additional access, the IBM MQ instance will normally be integrated into the wider messaging backbone, and may be administered by the central MQ team.

**Optimize for orchestration** : Container orchestration technology provides the ability to provide  **scalability and continuous availability**.  Although IBM MQ is infrequently the bottleneck, it is important to understand that  IBM MQ can utilize the same container scaling principles.

It is important to stress that the above is not a generic fit for all journeys. Each organization would need to look at the above aspects and  establish their own journey based on their current starting position and the benefits they want to realize. For instance the focus area of an organization, may be around team agility, and decoupling dependencies between teams and central IT, to provide agility.

The Foundational aspects are normally completed prior to the other two and the initial focus could be from a product modernization, and wanting to adopt containers for operational consistency and portability.

## Example MQ Containerization Journey

Lets walk through a potential MQ journey: 
An organization currently has a on-premise centric deployment, and they are concerned with moving to the public cloud. Instead they would like to enable their infrastructure to be more compatible so they can decide to move certain components in a gradual manner. They would also like to streamline their operations across software products, so software can be handled in a more standard approach, and simplify the infrastructure operations teams knowledge. As with all organizations they are always challenged to do more with less, and therefore any additional infrastructure optimization they can gain, while maintaining the isolation between deployments would be beneficial. They decide to call this phase Foundational. 

The second aspect, after foundational, will be enabling more team agility for the users of IBM MQ capabilities. Within the organization there is a strong IBM MQ Administration team that owns and maintains the messaging platform, however users are always complaining that they need more control and access, so they can react to their business requirements. This is all linked into a wider strategy of enabling the LOB or we could say internal development teams, to be independent and decoupled from the central teams. They decide to call this phase Decentralized.

Finally the organization is happy with the single active instance available for IBM MQ today, and it is not a performance bottleneck. However they are seeing rapid growth, and would like to assure that flexibility exists for the future to allow them to scale vertically and horizontally based on their needs. They decide to call this phase Optimized.



