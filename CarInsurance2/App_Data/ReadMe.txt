Hi,
The following is the Logic that i added to my code, But when i run it, i cannot see the changes on the Index.

Logic requirement: 1. You need to add in the logic to calculate the quote based on the conditions on the assignment page.
 
// Calculate quote based on given Insuree data
        private decimal CalculateQuote(Insuree insuree)
        {
            decimal quote = 50; // Base quote
            int age = DateTime.Now.Year - insuree.DateOfBirth.Year;

            // Age adjustments
            if (age <= 18)
                quote += 100;
            else if (age >= 19 && age <= 25)
                quote += 50;
            else
                quote += 25;

            // Car year adjustments
            if (insuree.CarYear < 2000 || insuree.CarYear > 2015)
                quote += 25;

            // Car make adjustments
            if (insuree.CarMake.ToLower() == "porsche")
            {
                quote += 25;
                if (insuree.CarModel.ToLower() == "911 carrera")
                    quote += 25;
            }

            // Speeding ticket adjustments
            quote += insuree.Speeding_Tickets * 10;

            // DUI adjustment
            if (insuree.DUI)
                quote *= 1.25m; // 25% increase

            // Coverage type adjustments
            if (insuree.CoverageType == CoverageType.Full)
                quote *= 1.5m; // 50% increase

            return quote;
        }

2. Add an Admin View for a site administrator to the Insuree Views. This page must show all quotes issued, along with the user's first name, last name, and email address.: Now Admin View logic is added to the code but it is as following as well: 

// Inside InsureeController

// GET: Insuree/Admin
public ActionResult Admin()
{
    // Retrieve all Insurees along with their quotes
    var insureesWithQuotes = db.Insurees.Select(insuree => new
    {
        insuree.FirstName,
        insuree.LastName,
        insuree.EmailAddress,
        insuree.Quote
    }).ToList();

    // Create a list of anonymous objects to pass to the view
    var viewModel = new List<object>();
    foreach (var item in insureesWithQuotes)
    {
        viewModel.Add(new
        {
            FirstName = item.FirstName,
            LastName = item.LastName,
            EmailAddress = item.EmailAddress,
            Quote = item.Quote.ToString("C") // Format quote as currency
        });
    }

    return View(viewModel);
}


