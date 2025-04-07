import { useEffect, useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";

export default function Dashboard() {
  const [signal, setSignal] = useState("Loading...");
  const [loading, setLoading] = useState(false);

  const fetchSignal = async () => {
    setLoading(true);
    try {
      const res = await fetch("http://localhost:8000/signal"); // Change to deployed backend URL
      const data = await res.json();
      setSignal(data.signal);
    } catch (error) {
      setSignal("Error fetching signal");
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchSignal();
    const interval = setInterval(fetchSignal, 60000); // Refresh every 1 minute
    return () => clearInterval(interval);
  }, []);

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100 p-4">
      <Card className="w-full max-w-md text-center shadow-xl">
        <CardContent className="space-y-4 p-6">
          <h1 className="text-2xl font-bold">Chiroms Trading Signal</h1>
          <div className="text-3xl font-mono">
            {loading ? "Fetching..." : signal}
          </div>
          <Button onClick={fetchSignal} disabled={loading}>
            Refresh Signal
          </Button>
        </CardContent>
      </Card>
    </div>
  );
}
