From Andreas Heinzmann, Mi 12.09.2018:
> Prefer enumeration classes over constants interfaces:
> Would suggest to use "real" enum classes instead
> We had suggested these centrally some time ago, see attachment
[Enumeration Classes - Object Pattern.pdf](Resources/Enumeration%20Classes%20-%20Object%20Pattern.pdf)

From Florian Hoffmann, Mi 12.09.2018:
> Their pattern treats each constant as an instance of the enum class and thus closely reflects Java's enums.
> I have no experience how this pattern "feels" in everyday usage, but like its design, so added both variants as alternatives for now.
> I could imagine that the object-oriented pattern is a little "heavier" because it needs more code, e.g. could require a generator that produces the enum classes.
> Would appreciate further comments and real-life comparisons between the two.
> Our thoughts are described in detail in
[Enumeration Classes - Constant Pattern.docx](Resources/Enumeration%20Classes%20-%20Constant%20Pattern.docx)

From Florian Hoffmann, Mi 12.9.2018:
> We should add a differntiation to the ABAP statement `ENUM` that was introduced after we invented those patterns.
> We identified some minor applications for `ENUM`, but its shortcomings are still so severe that we don't generally recommend it.
> It especially does not seem fit to replace those enum patterns.