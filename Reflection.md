# Reflection

Designing the domain model and class diagram for a complex system such as an intelligent database management system for hospitals or enterprise environments was both a challenging and enlightening experience. The task required critical thinking, attention to detail, and a deep understanding of object-oriented design principles. Below, I reflect on the key aspects of the process, including challenges faced, alignment with previous work, trade-offs made, and lessons learned.

1. Challenges Faced

A challenge was determining the appropriate level of abstraction for each domain entity. At first, I ended up developing excessively detailed entities that took on too many roles, which went against the Single Responsibility Principle. For instance, I had thought about combining Backup and Recovery into one class, viewing them as two facets of the same procedure. Nevertheless, this swiftly resulted in ambiguity when outlining their methods and duties, as each had its unique workflow and dependencies.

Another challenge was establishing relationships and their multiplicity. Grasping how many occurrences of one category connect with another necessitated reevaluating the business logic. For example, it wasn't clear right away that a single AIModel would handle the analysis of several databases, nor that any recovery process would consistently rely on a backup. Ensuring these associations were accurate involved reviewing the use cases to confirm real-world limitations

Method definitions were also challenging, especially in abstract domains like AI prediction and compliance reporting. These processes don’t always translate easily into clear object-oriented actions. I had to strike a balance between realism and practicality by defining representative methods such as predictFailure() or generateReport() that symbolized complex internal workflows.

2. Alignment with Previous Assignments
The class diagram aligns well with previous assignments, particularly the use cases and user stories. Each user story (such as “As an IT support staff, I want the system to restore data automatically”) was carefully mapped to specific classes (like Recovery and Backup) and their interactions. The AI-powered prediction, monitoring, and compliance requirements also directly informed the inclusion of the AIModel, AuditLog, and ComplianceReport classes.

The state diagram work helped define class methods more clearly. For example, the different states a database might enter—active, backed up, recovering—were represented via status attributes and methods like updateStatus() or initiateRecovery().

Requirements such as data protection (POPIA, HIPAA), real-time monitoring, and system reliability were encoded into our model through methods, relationships, and multiplicity. The modularity of the design supports scalability, security, and traceability, which were all key requirements.

3. Trade-offs Made
To keep the model manageable, I had to make several trade-offs. One key decision was to avoid deep inheritance. Although roles such as "Security Officer", "DBA", and "Compliance Auditor" could be subclasses of User, I kept User generic with a role attribute. This simplified the model while still preserving flexibility through role-based behavior.

Similarly, I chose composition over inheritance where possible, especially in areas like Backup and Recovery. These classes could have shared a parent "Process" class, but introducing that abstraction added unnecessary complexity. Instead, I kept their responsibilities isolated but interconnected through associations.

Another trade-off was not modeling encryption or monitoring frameworks in detail. These could be modeled as separate services or utility classes, but I opted to encapsulate them within methods to reduce diagram clutter.

4. Lessons Learned
- This project reinforced several key object-oriented design principles:

- Single Responsibility: Each class should handle one concept or function well. Splitting backup and recovery helped maintain clarity.

- Encapsulation: Methods like createBackup() or detectAnomaly() helped abstract complex logic behind simple interfaces.

- Modularity and Reusability: A modular design made it easy to imagine extending the system with new capabilities, such as support for cloud backups or predictive analytics.

- Clear Relationships: Using associations and multiplicity allowed me to mirror real-world workflows accurately and understand system behavior from both a technical and user-centric perspective.

Finally, I learned that domain modeling is iterative. It took multiple revisions, validation with use cases, and refinement of relationships to reach a model that was both robust and maintainable. This reflection process highlighted the importance of aligning design artifacts with system requirements while keeping the user experience and business logic in focus

