# CSharp and SQL Fundamentals
01. What is the purpose of a `namespace`?

  > | A namespace is a way to organize code into logical groups |

02. What is the difference between a `class` and an `interface`?

  > | A class is a blueprint for creating objects. It defines the properties and methods that an object of that type can have. An interface is a contract that defines the methods that a class must implement |

03. What is the method that returns an instance of a class, yet it has no return type?

  > | Constructor |

05. In the Car example what is the access modifier of the `Start()` method?

  ```c#
  abstract class Car
  {
    public string Start()
    {

      return "Vroooom";

    }
  }
  ```

  > | Public |

06. In the Car example what is `string` an indication of?

  > | Indication of the start method |

07. In the Car example what is `abstract` preventing?

  > | `abstract`  is preventing the Car class from being instantiated |

08. In a SQL table, what is the difference between information in a row and information in a column?

  > | information in a row represents a single record, while the information in a column represents a single attribute of that record |

09. Demonstrate the necessary SQL for creating a table called `characters` with the values `name, age, description` as strings, and an `int` id.

  > | CREATE TABLE characters (
  id INT PRIMARY KEY,
  name VARCHAR(255),
  age INT,
  description VARCHAR(255)
); |

10. In SQL how can you query more than a single table? Provide an example.

  > | ANSWER HERE |
