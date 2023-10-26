# Project-Data-Analysis-PowerBI-Netflix
This project shows the analysis of a Netflix dataset

The Advanced Editor in the Power Query Editor for this Dashboard is:

let
    Source = Csv.Document(File.Contents("D:\DATA SCIENCE\Projects for Profile\PBI Project-01 Netflix Dataset\netflix_titles.csv"), [Delimiter=",", Columns=12, Encoding=65001, QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {{"show_id", type text}, {"type", type text}, {"title", type text}, {"director", type text}, {"cast", type text}, {"country", type text}, {"date_added", type date}, {"release_year", Int64.Type}, {"rating", type text}, {"duration", type text}, {"listed_in", type text}, {"description", type text}}),
    #"Replaced Value" = Table.ReplaceValue(#"Changed Type", "", "Unknown", Replacer.ReplaceValue, {"director"}),
    #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value", "", "Not available", Replacer.ReplaceValue, {"cast"}),
    #"Replaced Value2" = Table.ReplaceValue(#"Replaced Value1", "", "Country not specified", Replacer.ReplaceValue, {"country"}),
    #"Filtered Rows1" = Table.SelectRows(#"Replaced Value2", each true),
    #"Replaced Value3" = Table.ReplaceValue(#"Filtered Rows1", ", South Korea", "South Korea", Replacer.ReplaceText, {"country"}),
    #"Replaced Value4" = Table.ReplaceValue(#"Replaced Value3", ", France, Algeria", "France / Algeria", Replacer.ReplaceText, {"country"}),
    #"Removed Errors" = Table.RemoveRowsWithErrors(#"Replaced Value4", {"date_added"}),

    // Create the "Duration (Minutes)" and "Duration (Seasons)" columns
    #"Added Custom Columns" = Table.AddColumn(#"Removed Errors", "Duration (Minutes)", each
        if Text.Contains([duration], "min") then
            Text.Start([duration], Text.PositionOf([duration], "min"))
        else
            null
    ),
    #"Added Custom Columns1" = Table.AddColumn(#"Added Custom Columns", "Duration (Seasons)", each
        if Text.Contains([duration], "Season") then
            Text.Start([duration], Text.PositionOf([duration], "Season"))
        else
            null
    )
in
    #"Added Custom Columns1"


Once the data transformation is being done, the data modeling is performed appropriately and then viuslaizations and formatting and styling is performed.

The dataset will be available in the repository

You can also download it from kaggle: https://www.kaggle.com/datasets/shivamb/netflix-shows/code
