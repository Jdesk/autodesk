---


---

<p><strong>Goal</strong>: Allow Redshift geo teams to have ability to publish geo specific content without having to spin up custom WP sites and subdomains.</p>
<p>The site would function the same except if there was one or more posts in a geo-location that has a geo-post(s) created on then you would see this content in addition to the standard .com content.</p>
<p><strong>Expected Behavior:</strong></p>
<ol>
<li>
<ul>
<li>Autodesk team grants geo user access to Wordpress website for .com OR</li>
</ul>
</li>
<li>
<ul>
<li>creates geo specific post (ex. Australia) on behalf of geo location</li>
</ul>
</li>
<li>
<ul>
<li>When post is created, the admin user is able to select one or more geotargeted locations* where this appears as featured content promoted on home and category pages, following the same layout as the existing site</li>
</ul>
</li>
<li>
<ul>
<li>When user accesses <a href="http://autodesk.com/redshift">autodesk.com/redshift</a>, their IP address will be automatically detected and routed through Akamai’s content targeting feature, where it is then used to determine eligibility for displaying featured geocontent as created in step 2.</li>
</ul>
</li>
<li>the domain will remain the same -  <a href="http://www.autodesk.com/redshift">www.autodesk.com/redshift</a></li>
</ol>
<p><strong>Assumptions:</strong></p>
<ul>
<li>Translations are handled within the custom post created.</li>
<li>SOUTH will have access to AD team that manages content targeting within Akamai</li>
<li>Categories and tags will remain the same across all potential geotargeted locations</li>
</ul>
<p>*IP/Targeting</p>
<ul>
<li>Our assumption is that Wordpress will learn of the IP address and either pass the country this IP is associated with to WP or we’ll have to create a lookup table matching this.</li>
</ul>

