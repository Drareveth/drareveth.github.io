# drareveth.github.io
:)
#### Chrome Extension Title Enumeration
Query's the extension ID's on a host to their respective title on the Chrome store
```bat
# Get the list of extension IDs from the previous query
$chrome = Get-ChildItem "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Extensions" | Select-Object Name
$extension_ids = $chrome | ForEach-Object { ($_ -split "\\")[-1] }
Write-Output "-----------------"
Write-Output "Extension ID's Identified"
foreach ($extension_id in $extension_ids) {
Write-Output "$extension_id"}
Write-Output "-----------------"

# Loop through each extension ID and look up the extension name
foreach ($extension_id in $extension_ids) {
    # Make a request to the Chrome Web Store API to retrieve the extension details
    $extension_id = $extension_id | Select-String -Pattern '(?<=Name=)[^}]*' | ForEach-Object { $_.Matches.Value }
    Write-Output "-----------------"
    Write-Output "Querying Extension ID: $extension_id"
    try { $url = "https://chrome.google.com/webstore/detail/$extension_id"
    $response = Invoke-WebRequest -Uri $url
    $titleRegex = "<title>(.+?)<\/title>" 
    $title = $response.Content | Select-String -Pattern $titleRegex | ForEach-Object { $_.Matches.Groups[1].Value }
    
    Write-Output "Extension ID: $extension_id"
    Write-Output ""
    Write-Output "CHROMESTORE QUERY RESULTS, Title: $title"

    Write-Output ""
    #Write-Output "RESPONSE TO CHROMESTORE QUERY FOR EXTENSION ID:"
    #Write-Output $response

    Write-Output "-----------------"
    Write-Output ""
    Write-Output "" }
    
    catch { "An error occurred."
    Write-Output "-----------------"
    Write-Output ""
    Write-Output "" }
    

}
```
