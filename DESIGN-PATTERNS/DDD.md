### CITY PLANING EXAMPLE
1. Strategic Design (The City Master Plan)
    -   This is about how the city is organized and how departments communicate.
        -   Ubiquitous Language (The City Registry):
            The Concept: Everyone—the Mayor, the Architects, and the Construction workers—uses the exact same terms.
            The Example: If the law calls it a "Zone," the blueprints shouldn't call it a "Region." If the Mayor calls it a "Residential Lot," the database code must name the class ResidentialLot.

        -   Bounded Context (The Specialized Districts):
            The Concept: Different districts use the same words differently.
            The Example: In the Traffic District, a "Bridge" is defined by weight limits and car lanes. In the Parks District, a "Bridge" is a scenic pedestrian walkway. They are two separate models because they serve different purposes.

        -   Context Mapping (The Infrastructure Connections):
            The Concept: How the Districts share information.
            The Example: A map showing that when the Housing District approves a new skyscraper, the Power Grid District must be notified to provide more electricity.  

2. Tactical Design (The Construction Blueprint)
    -   This is how you build the individual components inside one District (e.g., the Housing District).

        -   Entity (The Citizen):
            The Concept: Has a unique identity that persists even if details change.
            The Example: A Citizen has a Social Security Number (ID). Even if they change their name, job, or hair color, they are the same person in the eyes of the city.

        -   Value Object (The Building Material):
            The Concept: Defined only by its properties; it’s interchangeable and has no ID.
            The Example: A Red Brick. You don't name each individual brick. If a brick is "Red, Standard Size, Clay," it’s identical to any other brick with those specs. If it breaks, you swap it for another "Value Object" of the same type.

        -   Aggregate (The Apartment Complex):
            The Concept: A group of objects treated as one unit. The Aggregate Root is the gatekeeper.
            The Example: An Apartment Building is the Root. It contains Units, Elevators, and Lobbies. You don't let people buy a lobby directly; they must go through the Building's management (the Root) to interact with anything inside.

        -   Repository (The City Archives):
            The Concept: Where you "store" and "find" your building projects.
            The Example: The Property Archive. You ask it to "Find the building at 123 Main St." It retrieves the whole building (the Aggregate) for you to inspect or modify.
        
        -   Domain Service (The Civil Engineer):
            The Concept: Logic that doesn't belong to just one building or citizen.
            The Example: Calculated Property Tax. This requires looking at the building's value, the citizen's income, and current city laws. No single building knows how to calculate city-wide tax, so a "Tax Service" handles it.
            
        -   Domain Event (The Siren/Alarm):
            The Concept: A notification that something happened.
            The Example: "FireStarted". This event happens in the Housing District. The Fire Department (another context) hears the alarm and reacts, even though they didn't start the fire themselves.

    -   Summary Cheat Sheet
        -   Bounded Context: The District.
        -   Entity: The Citizen (Identity).
        -   Value Object: The Brick (No Identity).
        -   Aggregate: The Building (The Boundary).
        -   Domain Event: The Siren (The Notification).





### FOOTBALL TEAM MANAGMENT EXAMPLE
1. Strategic Design (The Club Management)
    - This is how the owners organize the entire organization.
        - Ubiquitous Language (The Playbook):
            The Example: If the Coach calls a play "The High Press," the players, the scouts, and the video analysts all use that exact term. No one says "the aggressive front defense" if the playbook says "High Press."

        -   Bounded Context (The Club Departments):
            The Example: In the Medical Dept, a "Player" is a biological body (heart rate, injuries). In the Marketing Dept, a "Player" is a brand (image rights, jersey sales). They are the same person, but the "Model" used to describe them is different in each room.

        -   Context Mapping (The Inter-office Memo):
            The Example: When the Medical Dept clears a player to play, they send a "Health Status" update to the Tactical Dept so the Coach knows who is available for the match.

2. Tactical Design (The Game Day Blueprint)
    -   This is how you manage the logic inside one department (e.g., The Team Operations).
        -   Entity (The Player):
            The Example: Lionel Messi. He has a unique ID (his registration number). Even if he changes his hair, his jersey number, or his position, he is still the same Entity.

        -   Value Object (The Score/Position):
            The Example: The Score (3-1). If you change the 3 to a 4, you have a completely new score. You don’t track the "identity" of a score; you only care about the numbers it holds.

        -   Aggregate (The Team):
            The Example: The Starting XI. The "Team" is the Aggregate Root. You don't just randomly swap a player's shoes; you tell the Team "Substitute Player X for Player Y." The Team (Root) ensures you still have exactly 11 players on the field (Business Rule).

        -   Repository (The Trophy Room / Archive):
            The Example: The Club Database. When the Coach needs to see last year’s stats, he asks the Repository to "Find Season 2025." It pulls the whole history out of storage.

        -   Domain Service (The Referee):
            The Example: Determining Offside. Being "Offside" isn't a property of just one player or just the ball. It’s a rule that involves the ball, the attacker, and the defenders. The Referee (Domain Service) calculates this logic because it doesn't "live" inside a single player.

        -   Domain Event (The Referee’s Whistle):
            The Example: "GoalScored." When the ball hits the net, the whistle blows. This event triggers other things: the scoreboard updates, the fans cheer, and the social media manager posts a tweet.