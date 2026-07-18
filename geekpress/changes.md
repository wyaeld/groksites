I want a 2 page version similar to https://stewartadvisory.co/ that has a top nav bar that just slides down to the appropriate part of the page.

Home
Services
FAQ
Get Started (action button)

There should be only a home page, and a /contact endpoint.


CTA Top Section

Book a call
No commitment.  20-minute call. Walk away with some clear plans.


Use Netlify forms via `netlify-forms` skill for building out the /contact endpoint

The form should have:
full name
work email
dropdown for which of the 4 services most interested in
a textarea for what is their main goal for the next 3 months.

# Edits-1
Remove the 'See how we can help' button

Replace this section:
The problem
Building software has never been easier — or riskier.

Change 'Risk you can sleep on' to say 'Risks managed'


Change the text under 'What technologies do you work with?' to say:
All mainstream technologies:  Python, NodeJS, Java, Ruby and others. AWS, Google Cloud and any other infrastructure support


Remove the 'How fast can we start?' item


Change
GeekPress is your technical partner for the AI era — from your first vibe-coded prototype to a hardened, fast-shipping product, 

to 

GeekPress is a technical partner for the AI era — from vibe-coded prototypes to hardened SaaS offerings, and the journey in between.

Add an llms.txt file that summarizes the entire offering and points agents at the contact page.

# Edits 2

Add a who were are section:

Anna
handles Content and Project Delivery
expert Practice Lead and IT Incident Manager with years of government and enterprise experience.
- clear communication
- attention to detail
- genuine car

Brad
handles Software, Infrastructure and Security
Over 20 years designing, building and running complex IT systems.
- endless curiousity
- pragmatic architect
- understands business

# Edits 3
drop the direct mailto link on the front page, only offer the contact form

Add Team into the topbar

Remove the A B headers from the Bios and just lead with the names