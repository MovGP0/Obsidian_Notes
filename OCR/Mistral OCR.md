# Using the Mistral OCR API in C# (LINQPad)

## Obtaining a Mistral API Key

To use the Mistral OCR API, you first need to create an account and get an API key. This involves a few steps:

1. **Sign up at Mistral AI** – Go to the Mistral AI platform (e.g. console.mistral.ai) and create an account or log in with an existing account.
2. **Enable API access** – In your account console, navigate to the **Workspace** or **Billing** section and add billing information (even for the free tier). Then choose a plan (there is a free “Experiment” plan or pay-as-you-go options) to activate API usage ([Quickstart | Mistral AI Large Language Models](https://docs.mistral.ai/getting-started/quickstart/#:~:text=,not%20share%20it%20with%20anyone)).
3. **Generate an API key** – In the console, go to the **API Keys** section and click “Create new key.” Give it an optional name and expiration if desired, then create it. **Copy the API key** and store it securely (you won’t be able to view it again once generated) ([How to get a Mistral API Key (2025) - Pickaxe Blog](https://www.pickaxeproject.com/post/how-to-get-a-mistral-api-key-2025#:~:text=Step%205%20,key)) ([Quickstart | Mistral AI Large Language Models](https://docs.mistral.ai/getting-started/quickstart/#:~:text=,not%20share%20it%20with%20anyone)).

Once you have your API key, you’ll use it to authenticate all calls to the Mistral OCR API.

## LINQPad Script Example

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Text.Json;
using System.Linq;

void Main()
{
    // Set the path to your images folder and ensure your API key is available
    string folderPath = @"C:\path\to\your\images";
    string apiKey = Environment.GetEnvironmentVariable("MISTRAL_API_KEY");
    if (string.IsNullOrEmpty(apiKey))
    {
        Console.WriteLine("API key not found. Please set the MISTRAL_API_KEY environment variable.");
        return;
    }

    // Initialize HttpClient for API calls
    using HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

    // Get all image files in the folder (adjust extensions as needed)
    var imageFiles = Directory.EnumerateFiles(folderPath)
                              .Where(f => {
                                  string ext = Path.GetExtension(f).ToLower();
                                  return ext == ".png" || ext == ".jpg" || ext == ".jpeg" || ext == ".bmp";
                              });
    foreach (string imagePath in imageFiles)
    {
        string fileName = Path.GetFileName(imagePath);
        Console.WriteLine($"Processing image: {fileName}");
        try 
        {
            // Read file bytes and convert to Base64 data URI
            byte[] imageBytes = File.ReadAllBytes(imagePath);
            string base64Image = Convert.ToBase64String(imageBytes);
            // Determine MIME type based on file extension
            string mimeType = imagePath.EndsWith(".png", StringComparison.OrdinalIgnoreCase) ? "image/png" : "image/jpeg";
            string dataUri = $"data:{mimeType};base64,{base64Image}";

            // Prepare JSON request payload
            var requestBody = new
            {
                model = "mistral-ocr-latest",
                document = new {
                    type = "image_url",
                    image_url = dataUri
                }
                // If you want to include image data in the response, you can add: include_image_base64 = true
            };
            string json = JsonSerializer.Serialize(requestBody);
            
            // Send HTTP POST request to Mistral OCR API
            var content = new StringContent(json, Encoding.UTF8, "application/json");
            HttpResponseMessage response = client.PostAsync("https://api.mistral.ai/v1/ocr", content).Result;

            if (!response.IsSuccessStatusCode)
            {
                // Log error and skip to next file
                string errorDetail = response.Content.ReadAsStringAsync().Result;
                Console.WriteLine($"OCR API error for {fileName}: {response.StatusCode} - {errorDetail}");
                continue;
            }

            string resultJson = response.Content.ReadAsStringAsync().Result;
            using JsonDocument doc = JsonDocument.Parse(resultJson);
            // Extract Markdown text from all pages in the result
            var pages = doc.RootElement.GetProperty("pages");
            StringBuilder markdownOutput = new StringBuilder();
            foreach (JsonElement page in pages.EnumerateArray())
            {
                string pageText = page.GetProperty("markdown").GetString();
                markdownOutput.AppendLine(pageText);
            }

            // Save the markdown output to a .md file with the same base name
            string mdPath = Path.Combine(folderPath, Path.GetFileNameWithoutExtension(fileName) + ".md");
            File.WriteAllText(mdPath, markdownOutput.ToString());
            Console.WriteLine($"✅ OCR result saved: {mdPath}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error processing {fileName}: {ex.Message}");
        }
    }
}
```

## Sources

- Mistral AI Documentation – _OCR and Document Understanding_ (API key setup and usage) ([Quickstart | Mistral AI Large Language Models](https://docs.mistral.ai/getting-started/quickstart/#:~:text=,not%20share%20it%20with%20anyone)) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=curl%20https%3A%2F%2Fapi.mistral.ai%2Fv1%2Focr%20%5C%20,))
- Mistral AI Documentation – _OCR API Reference_ (Authorization and request format) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=curl%20https%3A%2F%2Fapi.mistral.ai%2Fv1%2Focr%20%5C%20,)) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=%22model%22%3A%20%22mistral,true))
- Mistral AI Documentation – _OCR Usage Examples_ (image input methods and base64 data URI) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=document%3D%7B%20%22type%22%3A%20%22image_url%22%2C%20%22image_url%22%3A%20%22https%3A%2F%2Fmedia,33d404.jpg%22)) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=ocr_response%20%3D%20client.ocr.process%28%20model%3D%22mistral,%7D))
- Mistral AI Documentation – _File Upload for OCR_ (uploading files and using document URLs) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=curl%20https%3A%2F%2Fapi.mistral.ai%2Fv1%2Ffiles%20%5C%20,F%20file%3D%22%40uploaded_file.pdf)) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=ocr_response%20%3D%20client.ocr.process%28%20model%3D%22mistral,signed_url.url%2C%20%7D))
- Mistral AI Documentation – _Output Format_ (returns Markdown by default) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=,PDF%2C%20images%2C%20and%20uploaded%20documents)) ([OCR and Document Understanding | Mistral AI Large Language Models](https://docs.mistral.ai/capabilities/document/#:~:text=,at%20scale%20with%20high%20accuracy))
- Pickaxe Project Blog – _How to get a Mistral API Key_ (step-by-step key generation guide)
