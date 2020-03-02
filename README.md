# RFAdataset
Code to construct the RFA dataset.

# Purpose

These data are assembled for analysis required by the Regulatory Flexibility Act.  Fishing vessels (permits) are linked together, an industry determination is made, and firms are classified as small or large based on SBA guidelines.  Per SBA guidelines, we use revenue data in a single year to make the industry determination. Per SBA guidelines, we use 3 year trailing average to make the “small” determination.  

You should think *very carefully* about using this dataset for other purposes.

# Overview 
We use permit and ownership data from ap_year **YYYY** to link together permits into firms.  We use permit holdings on **June 1, YYYY** to construct the PLAN_CAT variables.  These take on the value of "1" if a permit held a “PLAN_CAT” and 0 otherwise. These may be useful is quickly determining the regulated entity.  We use dealer data from years **YYYY-3** through year **YYYY-1** to construct commercial revenues and VTR data combined with recreational survey data to construct for-hire revenues.   

Analysis required by the Regulatory Flexibility Act should use the Affiliate_id, year, and permit fields to correctly group fishing vessels into entities.  All other data is provided as a convenience.


# Warnings
1. Do not sum the affiliate revenue variables.  You will not get the total revenues.  If you want total revenues, you should either:
    1. Select the distinct AFFILIATE_ID and YEAR entries and SUM the affiliate revenue columns, or 
    2. Sum the value_permit, value_permit_finfish, value_permit_shellfish and value_permit_forhire columns

2. There is no guarantee that permits that were affiliated in a particular year were also affiliated in previous years.  For example, the fact that permits 123 and 456 were affiliated in 2013, does not imply that they were affiliated in 2012.  Once a group of permits is affiliated together, revenues for the trailing 3 years are combined and aggregated.  For example, if permits 123 and 456 were affiliated in 2013 but not in 2011 or 2012, the average revenues for 123 and 456 across the 2011-20123 period are summed when making a SBA size determination.  This is consistent with current SBA guidance.

3. When the dataset is generated for subsequent years, the affiliate id variables will change.  For example if permits 123 and 456 were affiliate_id =3 in 2013, that same grouping (if it even exists) is likely to have a different value of affiliate_id in 2014.  This is probably fine for RFA purposes.

# Examples
Please see the subfolder in "stata_code" for a few stata code samples.  You're on your own for SAS or R.

# References

## CFR Title 13, Part 121

§121.103   How does SBA determine affiliation?
(a) General Principles of Affiliation. (1) Concerns and entities are affiliates of each other when one controls or has the power to control the other, or a third party or parties controls 
	or has the power to control both. It does not matter whether control is exercised, so long as the power to control exists.
(2) SBA considers factors such as ownership, management, previous relationships with or ties to another concern, and contractual relationships, in determining whether affiliation exists.

(3) Control may be affirmative or negative. Negative control includes, but is not limited to, instances where a minority shareholder has the ability, under the concern's charter, by-laws, 
	or shareholder's agreement, to prevent a quorum or otherwise block action by the board of directors or shareholders.

(4) Affiliation may be found where an individual, concern, or entity exercises control indirectly through a third party.

(5) In determining whether affiliation exists, SBA will consider the totality of the circumstances, and may find affiliation even though no single factor is sufficient to constitute affiliation.

(6) In determining the concern's size, SBA counts the receipts, employees, or other measure of size of the concern whose size is at issue and all of its domestic and foreign affiliates,
	regardless of whether the affiliates are organized for profit.

(e) Affiliation based on common management. Affiliation arises where one or more officers, directors, managing members, or partners who control the board of directors and/or management
	of one concern also control the board of directors or management of one or more other concerns.

(f) Affiliation based on identity of interest. Affiliation may arise among two or more persons with an identity of interest. Individuals or firms that have identical or substantially 
	identical business or economic interests (such as family members, individuals or firms with common investments, or firms that are economically dependent through contractual
	or other relationships) may be treated as one party with such interests aggregated. Where SBA determines that such interests should be aggregated, an individual or firm may
	rebut that determination with evidence showing that the interests deemed to be one are in fact separate.

§121.104   How does SBA calculate annual receipts?
(c) Period of measurement. (1) Annual receipts of a concern that has been in business for three or more completed fiscal years means the **total receipts of the concern over its most recently completed three fiscal years divided by three**.
(d) Annual receipts of affiliates. 
	(1) The average annual receipts size of a business concern with affiliates is calculated by adding the  average annual receipts of the business concern with the average annual receipts of each affiliate.
	(2) If a concern has acquired an affiliate or been acquired as an affiliate during the applicable period of measurement or before the date on which it self-certified as small, the annual receipts used in determining size status includes the receipts of the acquired or acquiring concern. Furthermore, **this aggregation applies for the entire period 
	of measurement**, not just the period after the affiliation arose.


§121.107   How does SBA determine a concern's "primary industry"?

In determining the primary industry in which a concern or a concern combined with its affiliates is engaged, SBA considers the distribution of receipts, employees and costs of doing business among the different industries in which business operations 	occurred for the most recently completed fiscal year. SBA may also consider other factors, such as the distribution of patents, contract awards, and assets.

## NMFS small business size standards

NMFS has it's own size standards for commercial fishing.  As of August 19, 2019 the for-hire standard is $8M. The commerical fishing size standard is $11M.
