# Assignment-4-Graph-Databases

### Setup
First you must create a database in neo4j, call it what ever you like. 
Then add the csv file from the data folder, into the database you created neo4j imports folder.


### In the neo4j database browser


This is the querys to import the data and make the necessary nodes and relations, all queries have to be run separately.... 
we are sorry for the inconvenience if you want to test it out :/

```
WITH "file:///Sample_Game_1_RawEventsData.csv"
AS url
LOAD CSV WITH HEADERS FROM url AS row WITH row WHERE row["Type"] <> "PASS"
MERGE (p:Player {id:row["From"]})
MERGE (e:Event {id:row["Type"] + " Time: " + row["Start Frame"] +" " + row["End Time [s]"]})
SET p.team = row["Team"],
e.period = row["Period"],
e.StartTime = row["Start Time [s]"],
e.EndTime = row["End Time [s]"],
e.StartFrame = row["Start Frame"],
e.EndFrame = row["End Frame"],
e.StartX = row["Start X"],
e.EndX = row["End X"],
e.StartY = row["Start Y"],
e.EndY = row["End Y"],
e.Subtype = row["Subtype"];

```

```
//Create pass Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "PASS" match (pFrom:Player {id:row["From"]}), (pTo:Player{id:row["To"]}) with pFrom,pTo,row create (pFrom)-[r:pass]->(pTo)
SET r.period = row["Period"],
r.StartTime = row["Start Time [s]"],
r.EndTime = row["End Time [s]"],
r.StartFrame = row["Start Frame"],
r.EndFrame = row["End Frame"],
r.StartX = row["Start X"],
r.EndX = row["End X"],
r.StartY = row["Start Y"],
r.EndY = row["End Y"];
```

```
//Create Set Piece Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "CHALLENGE" match (e:Event {id:"CHALLENGE"+ " Time: " + row["Start Time [s]"] +" " + row["End Time [s]"]}), (pTo:Player{id:row["From"]}) with e,pTo,row create (e)-[r:CHALLENGE]->(pTo)

```

```
//Create SHOT Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "SHOT" match (e:Event {id:"SHOT"+ " Time: " + row["Start Time [s]"] +" " + row["End Time [s]"]}), (pTo:Player{id:row["From"]}) with e,pTo,row create (e)-[r:SHOT]->(pTo)
```

```
//Create RECOVERY Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "RECOVERY" match (e:Event {id:"RECOVERY"+ " Time: " + row["Start Time [s]"] +" " + row["End Time [s]"]}), (pTo:Player{id:row["From"]}) with e,pTo,row create (e)-[r:RECOVERY]->(pTo)
```

```
//Create CHALLENGE Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "CHALLENGE" match (e:Event {id:"CHALLENGE"+ " Time: " + row["Start Time [s]"] +" " + row["End Time [s]"]}), (pTo:Player{id:row["From"]}) with e,pTo,row create (e)-[CHALLENGE]->(pTo)

```

```
//Create BALL OUT Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "BALL OUT" match (e:Event {id:"BALL OUT"+ " Time: " + row["Start Time [s]"] +" " + row["End Time [s]"]}), (pTo:Player{id:row["From"]}) with e,pTo,row create (e)-[BALL OUT]->(pTo)
```

```
//Create FAULT RECEIVED Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "FAULT RECEIVED" match (e:Event {id:"FAULT RECEIVED"+ " Time: " + row["Start Time [s]"] +" " + row["End Time [s]"]}), (pTo:Player{id:row["From"]}) with e,pTo,row create (e)-[FAULT RECEIVED]->(pTo)
```

```
//Create FAULT CARD Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "CARD" match (e:Event {id:"CARD"+ " Time: " + row["Start Time [s]"] +" " + row["End Time [s]"]}), (pTo:Player{id:row["From"]}) with e,pTo,row create (e)-[CARD]->(pTo)
```

```
//Create BALL LOST Relation
LOAD CSV with headers FROM "file:///Sample_Game_1_RawEventsData.csv" AS row WITH row WHERE row["Type"] = "BALL LOST" match (e:Event {id:"BALL LOST"+ " Time: " + row["Start Time [s]"] +" " + row["End Time [s]"]}), (pTo:Player{id:row["From"]}) with e,pTo,row create (e)-[BALL LOST]->(pTo)
```

##### who is the most active player (in terms of passing and receiving the ball)?
With this query we see that player 21 for the away team, is the most active
```
// Most Active Player
MATCH ((a)-[r:pass]->(b))
WITH b, COUNT() as passCount
ORDER BY passCount desc LIMIT 1
RETURN passCount, b.id
UNION
MATCH ((b)-[r:pass]->(a))
WITH b, COUNT() as passCount
ORDER BY passCount desc LIMIT 1
RETURN passCount, b.id

```
#### who has had a central role in the match?

With this query we see that player 19 with 110 interactions.

```
// Most eventful Player
MATCH ((a)-[r]->(b))
WITH b, COUNT(*) as interactions
ORDER BY interactions desc LIMIT 1
RETURN interactions, b.id, b.team
```

#### which players have attempted to score?

With this query we see these palyer are trying to score... there is alot XD

```
With this query we see that player 19 with 110 interactions.
```

#### which team has kept the ball longer?



#### is there any close ‘societies’ between players (passing the ball to each other)?

```
```

#### how close is the connection between two specific players?

```
```




