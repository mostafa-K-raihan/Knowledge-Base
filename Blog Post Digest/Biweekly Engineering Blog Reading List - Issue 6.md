## Distance Service at Swiggy

Swiggy shares the design for its Distance Service - a service built on top of [Google Distance Matrix API (GDMA)](https://link.mail.beehiiv.com/ss/c/FTOuKoDkZylxfKbYvc7TI6-9EZJfNULe0z1cU--XkqM8TzUsEcup8QTBZDRQ5Un24Z0vNT4BI1S4IEBVPNx5YH7UJpuFoBV1GYR8InUrKRpukQ55tr4gC_eaiCADTMfw/3t3/ivwSZRGiRHWdqFEqHfOuyQ/h5/0S4-hg72sai7vOn3OdKbhY0ffg5Kg_xE_kpnhLX8III) that works as a unified API layer for providing distance and time-to-travel between two points. GDMA is Google service which is capable of providing distance and travel time between an _origin_ and a _destination_.

Initially, Swiggy started with using GDMA directly. Every system needing distance and time-to-travel data would have their own code calling GDMA. And a shared API key was used. The API had a usage limit - distance and travel time for maximum 1000 origin and destination pairs can be fetched in one second. If this limit was reached, Swiggy would fallback to [Open Street Maps (OSM)](https://link.mail.beehiiv.com/ss/c/-MssP2pBPPMbghFB2AyfpViGxovVggdp754FU97oY1CQTiJUbdKTuKWgaI0b6Mzu/3t3/ivwSZRGiRHWdqFEqHfOuyQ/h6/PL4fSZrgxQfGLcvUGlW-bbb_8UVP3nYnsdsKnAVIVgY) to fetch the distance information. But OSM's accuracy was not good enough.

A reasonable strategy here is to cache the data received from GDMA on Swiggy's side. This is why the Distance Service was built. Instead of every system going to GDMA for the data, it would ask this new service. The service is responsible for fetching distance and travel time from GDMA as well as caching the response in an in-memory database called [Aerospike](https://link.mail.beehiiv.com/ss/c/YxzDJI_rADt-h6huEBw1Ql5rTVpbHRNgLzNIjK4vgAg/3t3/ivwSZRGiRHWdqFEqHfOuyQ/h7/1LcVxLfpPnqdI5X0E1-NZYrZs06VYtZalRgOolk9OSg).

The article has interesting details, like how granular origin and destination points were required (thanks to [Geohash](https://link.mail.beehiiv.com/ss/c/-MssP2pBPPMbghFB2AyfpWj1kwewr0Isk8efz_64mrSaIbxfDYCz-rFPGDbjjWbaNYWyN4Y4nibNQT_TLApC9A/3t3/ivwSZRGiRHWdqFEqHfOuyQ/h8/f0G_9EFvqJyAGXNLl3Lh8s4vzsU92mT2rbMO4gSQuGs)), and how they throttled the requests to GDMA.


## JSON Logging System at Pinterest

Pinterest built an end-to-end JSON logging system to visualize client side app performance in real-time. This is called _JSON_ logging, as the data is schemaless with key-value pairs.

At a high level, JSON logs from Pinterest apps are batch uploaded to the log service. The service relays the messages to [Singer](https://link.mail.beehiiv.com/ss/c/kKZmnZzqHRiCqeWAAd3Meg_Lo1RcHlDHzwSyvH_k2LHYhlqjxdIFlX8HeGh90EP36uUEQACLm5Nr91oK21g8XraMUGr7TVOPDTjLN0prgqBjQ1O3xKDZEgXugUT-GM5tS2YCI9D7BpIiNusmFHyWvNTSbLqi1TADgykc8mVviAMrXaIprEaY_n0uWk76IYQg/3t3/ivwSZRGiRHWdqFEqHfOuyQ/h10/VNPX-y_Uis4LVYtF_oTFguPU3pOaqYWPLEgf43fVOJw) - Pinterest's side-car for Kubernetes that manages log uploads to Kafka. These logs are read by [Logstash](https://link.mail.beehiiv.com/ss/c/-MssP2pBPPMbghFB2AyfpVWchZt6RSdjf6D7ZkAiT-NsXFCUK6yuXqOWd13zGrjy/3t3/ivwSZRGiRHWdqFEqHfOuyQ/h11/1pW26Lem4FfpwEs6JHexUQcGLZh1OpvCsWp8GccdNUs) and sent to [OpenSearch](https://link.mail.beehiiv.com/ss/c/GQ7ailYNmvg8m_n1t4j11dIA9IrqPlDloVauNtZm6uU/3t3/ivwSZRGiRHWdqFEqHfOuyQ/h12/fcdId4yKh2Lj9zN7U2wyU3uJBsww7-cmT0HQYv5tgTg) for visualizing. The system also supports metrics and alerting on top of the logs for better observability.

Logging systems like this make the life of developers easier. Developers at Pinterest adopted the system to visualize network and app errors, performance insights, code execution paths, etc.