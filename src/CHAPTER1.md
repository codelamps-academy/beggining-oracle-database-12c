# CHAPTER 1
## Relational Database Systems and Oracle
The focus of this book is writing SQL in Oracle, which is a relational database management system. This first chapter provides a brief introduction to relational database systems in general, followed by an introduction to the Oracle software environment. The main objective of this chapter is to help you find your way in the relational database jungle and to get acquinted with the most important database terminology.
The first three sections discuss the main reasons for automating information systems using databases, what needs to be done to design and build relational database systems, and the various components of a relational database management system. The following sections go into more depth about the theoretical foundation of relational database management systems.
This chapter also gives a brief overview of the Oracle software environment : the components of such an environment, the characteristic of the components, and what you can do with those components.
The last section of this chapter introduction seven sample tables, which are used in the examples and excercise throughout this book to help you develop yiour SQL skills. In order to able to formulate and execute the correct SQL statement, you'll need to understand the structures and relationalships of these tables. This chapter does not cover object-relational database features. In Chapter 12 you will find information about Oracle features in that area.

## 1.1 Information Needs and Information Systems
Organization have business objectives. In order to realize those business objectives, many decisions must be made on a daily basis. Typically, a lot information is needed to make the right decisions; however, this information is not always available in the appropriate format. Therefore, organizations need formal systems that will allow them to produce the required information, in the right format, at the right time. Such systems are called information systems. An information system is a simplified reflection ( a model ) of the real world within the organization. Information systems don't necessarily need to be automated - the data might reside in card files, cabinets, or other physical storage mechanisms. This data can be converted into the desired information format using certain procedures or actions. In general, there are two main reasons to automate information systems :
 - Complexity : The data structures or the data processing procedures become too complicated
 - Volume : The volume of the data to be administered becomes too large
If an organization decides to automate an information system because of complexity, volume, or both, it typycally will need  to use some database technology

### The main advantage of using database technology are as follows :
- Accessibility : Ad hoc data-retrieval functionallity, data-entry and data-reporting facilities, and concurrency handling in a multiuser environment
- Availability : Recovery facilities in case of system crashes and human errors
- Security : Data access control, privileges, and auditing
- Manageability : Utilities to efficiently manage large volumes of data

When specifying or modeling information needs, it is a good idea to maintain a clear separation between information and application. In other words, we separate the following aspects :
- What : The information content needed. This is the logical level and it represent the information
- How : The desired format of the information, the way that the result can be derived from the data stored in the information system, the minimum performance requirements, and so on. This is the physical level and it represent the application

Database systems such as Oracle enable information system users and designers / developers to maintain this separation between the "what" and the "how" aspect, allowing users of such systems to concentrate more on the firs aspect and less on the second. This is because database system implementations are based on the relational model. The relational model is explained later in this chapter, in Section 1.4 through 1.7

1.2 Database Design
One of the problems with using traditional third-generation programming languages (such as COBOL, Pascal, Fortran, and C) is the ongoing maintenance of existing code, because these languages don't separate the "what" and the "how" aspects of information needs. That's why programmers using those language sometimes spend more than 75% of their precious time on maintenance of existing programs, leaving little time for them to build new programs. When using database technology, organization usually need many database applications to process the data residing in the database. These database applications are typically developed using forth - or flifth - generation application development environments, which significantly enhance productivity by enabling users to develop database application faster while producing appliations with lower maintenance costs. However, in order to be successful using these fourth-and fifth generation application development tools, developers must start thinking about the structure of their data first. It is very important to spend enough time on designing the data model before you start coding your applications. Data model mistakes discovered in a later stage, when the system is already in production, are very difficult and expensive to fix.

### Entities and Attributes
In a database, we store facts about certain objects. In database jargon, such objects are commonly referred to as entities. For each entity, we are typically interested in a set of observable and relevant properties, commonly referred to as attributes.
When designing a data model for your information system, you begin with two questions :
- Which entities are relevant for the information system?
- Which attributes are relevant for each entity, and which values are allowed for those attributes?
We'll add a thrid question to this list before the end of this chapter to make the list complete.
For example, consider a company in the information technology training business. Example of relevant entities for the information system of this company could be course attendee, classroom, instructor, registration, confirmation, invoice, course, and so on. An example of a partial list of relevant attributes for the entity COURSE_ATTENDEE could be the following :
- Registration Number
- Name
- Address
- City
- Date of Birth
- Age
- Gender

For the COURSE entity, the attribute list might include attribute items such as :
- Title
- Duration
- Price
- Frequency
- Maximum number of attendees

### NOTE : THERE ARE MANY DIFFERENT TERMINOLOGY CONVENTIONS FOR ENTITIES AND ATTRIBUTES, SUCH AS OBJECTS, OBJECT TYPES, TYPES, OBJECT OCCURENCES, AND SO ON. THE TERMINOLOGY ITSELF IS NOT IMPORTANT, BUT ONCE YOU HAVE MADE A CHOICE, YOU SHOULD USE IT CONSISTENTLY

## Generic VS Specific
The difference between generic versus specific is very important in database design. For example, common words in natural language such as book and course have both generic and specific meaning. In spoken language, the precise meaning of these words is normally obvious from the context in which they are used. When designing data models, you must be very careful about the distinction between generic and specific meanings of the same word. For example, a course has a title and a duration (generic), while a specific course offering has a location, a start date, a certain number of attendees, and an instructor. A specific book on the shelf might have your name and purchase date on the inside cover page, and it might be full of your personal annotations. On the other hand, a generic book has a title, an author, a publisher, and an ISBN code. This means that you should be careful when using words like course and book for database entities, because they could be confusing and suggest the wrong meaning. Moreover, we must maintain a clear separation between an entity itself at the generic level and a specific occurrence of that entity. Along the same line, there is a difference between an entity attribute (at the generic level) and a specific attribute value for a particular entity occurrence

## Redundancy
There are two types of data : base data and derivable data. Base data is data that cannot be derived in any way from other data residing in the information system. It is crucial that base data is stored in the database. Derivable data can be deduced (for example, with a formula) from other data. For example, if we store both the age and the date of birth of each course attendee in our database, these two attributes are mutually derivable - assuming that the current date is available at any moment. Actually, every question issued against a database result in derived data. In other words, it is both undesirable and not reasonable to store all derivable data in  an information system. Storeage of derivable data is referred to as redundancy. Another way of defining redundancy is storage of the same data more than once. Sometimes, it makes sense to store redundant data in a database; for example, in cases where response time is crucial and in cases where repeated computation or derivation of the desire data would be too time-consuming. But typically, storage of redudant data in a database should be avoided. First of all, it is a waste of storage capacity. However, that's not the biggest problem, since terabytes of disk capacity can be bought for relatively low prices these days. The challenge with redundant data storage lies in its ongoing maintenance. With redundant data in your database, it is difficult to process data manipulation correctly under all circumstances. In case something goes wrong, you could end up with an information system containing internal contradictions. In other words, you could have inconsisten data. Therefore, redundancy in an information system may result in ongoing consistency problems. When considering the storage of redundant data in an information system, it is important to distinguish two types of information systems :
- Online transaction processing (OLTP) systems, which typically have continuous data changes and high volume
- Decision support systems (DDS; often referred to as data warehouses), which are mainly, or even exclusively, used for data retrieval and reporting, and are loaded or refreshed at certain frequencies with data from OLTP systems
In DSS systems, it is common practice to store a lot of redundant data to improve system response times. Retrieval of stored data is typically faster than data derivation, and the risk of inconsistency, although present for load and update of data, is less likely because most DSS systems are often read-only from the end user's perspective

## Consistency, Integrity, and Integrity Constraints
Obviously, consistency is a first requirement for any information system, ensuring that you can retrieve reliable information from that system. In other words, you don't want any contradictions in your information syste. For example, suppose we derive the following information from our training business information system : 
- Attendee 6749 was born on February 13, 2093
- The same attendee 6749 appears to have geng Z
- There is another, different attendee with the same number 6749
- We see a course registration for attendee 8462, but this number does not appear in the administration records where we maintain a list of all attendees.
In none of the above four cases is the consistency at stake; the information system is unambigours in its statements. Nevertheless, there is something wrong because these statements