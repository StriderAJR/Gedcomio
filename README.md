# Gedcomio

A .Net Standard library for loading, saving, working with and analysing family trees stored in the GEDCOM format.

Thank you to David A Knight who developed Gedcom.Net and Ryan O'Neill who developed GeneGenie project from which this project was forked.

## Basic usage

If you would like to see a specific sample **please let us know what you want via [Twitter (@ryanoneill1970)](https://twitter.com/ryanoneill1970) or create an issue in GitHub**.

Check the sample project out for working code, basic operations are;

### Loading a tree

To load a tree into memory use the following static helper.
```cs
    var gedcomReader = GedcomRecordReader.CreateReader("Data\\presidents.ged");
```
There are other variants of this helper and non static methods that allow you to specify additional parameters such as encoding.

You'll want to make sure that the file you just read was parsed OK and handle any failures;
```cs
    if (gedcomReader.Parser.ErrorState != Parser.Enums.GedcomErrorState.NoError)
    {
        Console.WriteLine($"Could not read file, encountered error {gedcomReader.Parser.ErrorState}.");
    }
```
### Querying the tree
```cs
    Console.WriteLine($"Found {db.Families.Count} families and {db.Individuals.Count} individuals.");
    var individual = db
        .Individuals
        .FirstOrDefault(f => f.Names.Any());

    if (individual != null)
    {
        Console.WriteLine($"Individual found with a preferred name of '{individual.GetName()}'.");
    }
```
### Adding a person to the tree
```cs
    var individual = new GedcomIndividualRecord(db);

    var name = individual.Names[0];
    name.Given = "Michael";
    name.Surname = "Mouse";
    name.Nick = "Mickey";

    individual.Names.Add(name);

    var birthDate = new GedcomDate(db);
    birthDate.ParseDateString("24 Jan 1933");
    individual.Events.Add(new GedcomIndividualEvent
    {
        Database = db,
        Date = birthDate,
        EventType = Enums.GedcomEventType.Birth
    });
```
### Saving the tree
```cs
    GedcomRecordWriter.OutputGedcom(db, "Rewritten.ged");
```
