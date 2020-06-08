<h1> Best Practices </h1>
<div class="cooked"><p>Looking for some of the best practices we follow when building in LookML? Here are some helpful tips:</p>

<h3>User Guide</h3>

Your resource for getting the most out of Looker.
<br>
<br>
<a style="padding: 15px 20px; color: #fff; text-transform: uppercase; background-color: #7e68dd; border-radius: 5px;" href="https://looker.com/guide/view">
View
</a>
  - Learn how to use Looker to view dashboards, reports, and more.


<br />
<br><br />

<a style="padding: 15px 20px; color: #fff; text-transform: uppercase; background-color: #7e68dd; border-radius: 5px;" href="https://looker.com/guide/build">
Build
</a>
 - Learn how to pivot and filter data, and share dashboards with your stakeholders.
<br />
<br><br />

<a style="padding: 15px 20px; color: #fff; text-transform: uppercase; background-color: #7e68dd; border-radius: 5px;" href="https://looker.com/guide/develop">
Develop
</a>
 - Learn how to connect to your database and get started writing LookML.
<br><br />

<br>
<h3>Naming Convention</h3>

<ul>
<li>Follow Looker <a href="https://discourse.looker.com/t/naming-fields-for-readability/712">naming conventions<span class="badge badge-notification clicks" title="134 clicks">134</span></a>:<ul>
<li>Name measures with aggregate function or common terms. <code>total_[FIELD]</code> for sum, <code>count_[FIELD]</code>, <code>avg_[FIELD]</code>, etc. </li>
<li>Name ratios descriptively. For example, “Orders Per Purchasing Customers” is clearer than “Orders Percent.”</li>
<li>Name yesno fields clearly: “Is Returned” instead of “Returned”</li>
</ul>
</li>
<li>Avoid the the words <code>date</code> or <code>time</code> in a dimension group because Looker appends each timeframe to the end of the dimension name: <code>created_date</code> becomes <code>created_date_date</code>,<code>created_date_month</code>, etc. Simply use<code>created</code>which becomes<code>created_date</code>,<code>created_month</code>, etc.<br>(More on <a href="https://discourse.looker.com/t/timeframes-and-dimension-groups-in-looker/247">timeframes<span class="badge badge-notification clicks" title="22 clicks">22</span></a>)</li>
</ul>

<h3>Project Organization</h3>

<ul>
<li>Try to keep one project per connection, with multiple models in each project if they share Views. Use multiple projects per connection when complete isolation of both Models and Views is necessary between git repository and developer teams. </li>
<li>Review the <a href="http://www.looker.com/docs/reference/model-params/include"><code>include:</code><span class="badge badge-notification clicks" title="38 clicks">38</span></a> statements, remove Views from models if unnecessary or use the wildcard. For example <code>include: “user*.view.lookml”</code> or  <code>include: “user*.view”</code> [New LookML] if several user Views (named user_order_facts, users, user_address…) are needed in the model.</li>
<li>Describe what questions each Explore can answer in the “Documentation” section, using one or many markdown LookML document within each project.</li>
</ul>

<h3>Model Building</h3>

<ul>
<li>Envision entire model from high level. Identify the key subject areas users will want to Explore and select Views to include. Joining many to one from the most granular level typically provides the best query performance. </li>
<li>Comment out extraneous auto-generated Explores in the Model file (using ‘#’ syntax) to reduce clutter when developing, such as those on dimension tables. Explores can be un-commented later as necessary.</li>
<li>Use the fewest number of Explores possible that allows users to easily get access to the answers they need. Consider splitting out into different Models for different audiences. The optimal number of Explores is different for every business, however many Explores tend to be confusing for the end user. </li>
<li>Organize Explores across multiple Models to help the end-user find the correct Explore as easily as possible.</li>
<li>Limit Access Filter Fields to a different model. Use LookML dashboards to make one dashboard work across several models (i.e. remove any <code>model</code> parameters from dashboard elements). More on these <a href="http://www.looker.com/docs/reference/explore-params/access_filter_fields">here<span class="badge badge-notification clicks" title="49 clicks">49</span></a> and <a href="https://discourse.looker.com/t/access-filter-fields/267">here<span class="badge badge-notification clicks" title="35 clicks">35</span></a>.</li>
</ul>

<h3>Explore Design</h3>

<ul>
<li>Limit joins in the Explore to only what is necessary to not overwhelm the user when seeing Views in the field picker. </li>
<li>Use the <a href="http://www.looker.com/docs/reference/explore-params/fields-for-join"><code>fields:</code><span class="badge badge-notification clicks" title="36 clicks">36</span></a> parameter for each join or at the Explore level to limit the number of dimensions, measures and filters in the field picker. </li>
<li>Add a short description to each Explore to specify the purpose and audience using the <code>description:</code> parameter.</li>
<li>Avoid joining one View into many Explores unless necessary. If the View has fields that scope to other Views this can cause LookML errors and confuse front-end users. Use the <code>fields:</code> parameter to limit this if possible. This also warrants using several View extensions to repeat core fields and add more unique fields for specific Explores. </li>
</ul>

<h3>Join Design</h3>

<ul>
<li>Use <code>sql_on</code> over <code>foreign_key</code> for better readability and LookML flexibility.<br>Write joins where the base View goes on left to match the relationship parameter, for easy readability.</li>
<li>Use <code>${}</code> unless referring to a date.  For date joins use raw sql (or timeframe <a href="https://discourse.looker.com/t/using-the-raw-timeframe-3-32/1549"><code>raw</code><span class="badge badge-notification clicks" title="13 clicks">13</span></a> on Looker 3.32+) to avoid cast/convert_tz in join predicates. However, avoid joining on concatenated primary keys declared in Looker - Join on the base fields from the table for faster queries. More on referencing fields <a href="https://discourse.looker.com/t/how-to-reference-views-and-fields-in-lookml/179">here<span class="badge badge-notification clicks" title="23 clicks">23</span></a>.</li>
<li>Always define a relationship using the <a href="http://www.looker.com/docs/reference/explore-params/relationship"><code>relationship:</code> parameter<span class="badge badge-notification clicks" title="36 clicks">36</span></a> to ensure correct aggregates are produced. </li>
<li>Hard code complex join predicates into map tables (PDTs) to enable faster joins.</li>
</ul>

<h3>View Design</h3>

<ul>
<li>Consider the naming, and familiarity of the end user when writing Views. See <a href="https://discourse.looker.com/t/making-views-user-friendly/1328">Making Views User Friendly<span class="badge badge-notification clicks" title="119 clicks">119</span></a>.</li>
<li>Declare a <a href="http://www.looker.com/docs/reference/field-reference#primary_key"><code>primary_key</code><span class="badge badge-notification clicks" title="15 clicks">15</span></a> in every View on the field that defines unique rows to ensure accuracy. More on why <a href="https://discourse.looker.com/t/why-create-lookml-primary-keys/1568">here</a>.</li>
<li>Use the <a href="http://www.looker.com/docs/reference/field-reference#hidden"><code>hidden:</code><span class="badge badge-notification clicks" title="5 clicks">5</span></a> parameter on dimensions that will never be used to avoid confusion (such as join Keys/IDs and compound primary keys, or those designed solely for application use.)</li>
<li>Describe fields with the description parameter (<code>description: ‘my great description’</code>). Typical database column names are not descriptive enough for end users. </li>
<li>Group together types of measures in the View in a consistent manner. This is stylistic but helps others read your model: Grouping by underlying field (counts then sums then averages etc. ) or all measures together by type (counts together, sums together, etc. ) are common methods. </li>
<li>Use <a href="http://www.looker.com/docs/reference/view-params/sets">sets<span class="badge badge-notification clicks" title="52 clicks">52</span></a> to organize groups of fields. This makes it easier to remove fields from Explores and makes the model more maintainable.</li>
<li>Replace count measures of joined-in tables with <code>count distinct</code> of the foreign key from the Explore, to avoid unnecessary joins (i.e. user_count = count(distinct orders.user_id)</li>
<li>Use <a href="http://www.looker.com/docs/reference/explore-params/view_label-for-join"><code>view_label</code><span class="badge badge-notification clicks" title="20 clicks">20</span></a> and <a href="https://looker.com/docs/reference/field-params/group_label"><code>group_label</code><span class="badge badge-notification clicks" title="8 clicks">8</span></a> parameters to consolidate dimensions and measures from multiple joined Views that fall under the same category. </li>
</ul>

<h3>PDT usage</h3>

<ul>
<li>Choose the parameter <code>sql_trigger_value</code> over <code>persist_for</code> when data should be ready the first time someone runs an explore or on a schedule. More on why <a href="https://discourse.looker.com/t/differences-between-sql-trigger-value-and-persist-for/479">here<span class="badge badge-notification clicks" title="34 clicks">34</span></a>.</li>
<li>Evaluate your <code>sql_trigger_value</code> schedules such that tables are not building during business hours/replication processes/peak usage times. Trigger the tables late in the night or early in the morning, after ETL is expected to be completed. </li>
<li>Always define indexes/distkeys/sortkeys to improve query performance.</li>
</ul>

<h3>Dashboards Organization/Design</h3>

<ul>
<li>Use the <a href="https://discourse.looker.com/t/drill-using-a-sparkline-or-other-images/910"><code>html</code><span class="badge badge-notification clicks" title="55 clicks">55</span></a> or <a href="https://looker.com/docs/reference/field-params/links"><code>links</code><span class="badge badge-notification clicks" title="14 clicks">14</span></a> parameter in a field to link dashboards (or internal sites) from measures and dimensions</li>
<li>Single value visualizations are great for highlighting high level KPIs, these do well on the top of the dashboard. </li>
</ul></div>
