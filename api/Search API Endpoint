export default async function handler(req, res) {
    const { q, timeRestrict } = req.query;

    if (!q) {
        return res.status(400).json({ error: "Search query is required" });
    }

    // These are never exposed to the browser
    const apiKey = process.env.GOOGLE_API_KEY;
    const cx = process.env.GOOGLE_CX_ID;

    if (!apiKey || !cx) {
        console.error("Missing API Credentials in Vercel settings.");
        return res.status(500).json({ error: "Server configuration missing" });
    }

    try {
        const apiUrl = `https://www.googleapis.com/customsearch/v1?key=${apiKey}&cx=${cx}&q=${encodeURIComponent(q)}&dateRestrict=${timeRestrict || ''}`;
        
        const response = await fetch(apiUrl);
        const data = await response.json();

        // Pass Google's error to the frontend if one occurs
        if (data.error) {
            return res.status(500).json({ error: data.error.message });
        }

        // Return the clean data to the frontend
        return res.status(200).json(data);
    } catch (error) {
        console.error("Fetch Error:", error);
        return res.status(500).json({ error: "Internal Server Error" });
    }
}
