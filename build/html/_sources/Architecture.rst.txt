Architecture
============

BLIS is based on the three-tier architecture; interpreted as the user interface (presentation layer), application tier/business rules layer and the data storage layer. The presentation layer is made up of a RESTful (HTTP) API built with Vue JS and accessed by end users through web browsers. BLIS's functionality is exposed to end users through the APIs. This opens up the system to other platforms, including mobile apps that may use the API approach to provide integration and provide functionality. The application tier which is the main business logic is housed on a central server. The underlying architecture is independent of Operating System. Both Windows and Unix based Operating Systems can be used to host BLIS.

All data is stored in a central relational MySQL database which is also the final tier. All end-points access the same data in their operation.