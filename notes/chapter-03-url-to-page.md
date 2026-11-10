# Chapter 3: What happens when you type a URL

## The journey

Step 1. The browser checks its own DNS cache.

Step 2. The operating system checks its cache and the hosts file.

Step 3. The recursive resolver (the ISP's, or 8.8.8.8) is asked. If it has the answer cached, the story ends here.

Step 4. The resolver asks a root server, which says who runs .com.

Step 5. The resolver asks the .com TLD server, which says who runs google.com.

Step 6. The resolver asks Google's authoritative server, which gives the IP and a TTL.

Step 7. The answer is cached and returned. Then come the TCP handshake, the TLS handshake, the HTTP request and the HTTP response.

## Key terms

IP address is the real address of a computer. A domain name is the human-friendly name.

The hosts file is a local text file (/etc/hosts) that maps names to IPs by hand and overrides DNS.

A recursive resolver hunts for the answer on your behalf and caches it.

8.8.8.8 is Google's public recursive resolver. 1.1.1.1 is Cloudflare's.

The root knows who runs each ending, a TLD server knows who runs each name, and the authoritative server owns the real record.

TTL is the number of seconds an answer may be cached.

## Terminal exercise

dig google.com returns an A record (the IP) and a TTL that counts down as the resolver's cache ages.

dig @8.8.8.8 google.com returns the same IP with a different TTL, because each resolver has its own cache.

dig +trace may stop after one line on a company network, which only allows DNS through its own resolver. Running dig NS . then dig NS com. then dig NS google.com lists each level instead.

## Questions

Why can't a laptop ask the root server directly?

The root does not know the IP of google.com, only who runs .com. Billions of devices asking the root would also overload it, so resolvers do the legwork and cache answers for many users.

What is the difference between a recursive resolver and an authoritative server?

A resolver hunts for answers and caches them. An authoritative server owns the real records for its domains and answers only for those.

A DNS record was changed an hour ago and some users still see the old site. Why?

Caching. Browsers, operating systems and resolvers keep the old IP until its TTL expires. Lowering the TTL before a planned change shortens this window.

## DSA status

Contains Duplicate and Contains Duplicate II: solved.

Contains Duplicate III (LeetCode 220): parked. Idea to learn: a sliding window plus buckets of width t + 1.
