# Entity reflection
Represents a single entity class information and every entity have it automatically. It stores list of all [property reflections](#property-reflection) defined in docblok.

> Every new reflection instance may have negative impact on performance, since the architecture of library is designed to work with it as carefully as possible, please avoid creating it again if not necessary.

# Property reflection
Stores informations about entity's [property](entity#property).