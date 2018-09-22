# Core0

Core0 is a Cisco 4500 series switch serving as the central switch and router for the CSL Machine Room. It runs in native mode with IOS on both the SP \(Switch Processor\) and RP \(Route Processor\).

One important consideration when connecting systems to Core0 is that each module has six 1Gb links to the switch backplane which are each linked to groups of adjacent ports. Therefore, a server's connections should be offset by four \(for a 24 port module\) or eight \(for a 48 port module\) ports to balance them across multiple backplane links. This concern does not apply to module 2 which has only one port for each backplane link.

## Technical Specifications

<table>
  <thead>
    <tr>
      <th style="text-align:left">Switch Model</th>
      <th style="text-align:left">Cisco Catalyst 4506</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Type</td>
      <td style="text-align:left">Modular</td>
    </tr>
    <tr>
      <td style="text-align:left">Ports</td>
      <td style="text-align:left">
        <p>Module 1: 2x 10Gb X2 + 4x 1Gb SFP</p>
        <p>Module 2: 6x 1Gb GBIC</p>
        <p>Module 3: 24x 1Gb Copper</p>
        <p>Module 4: 48x 1Gb Copper</p>
        <p>Module 5: 48x 100Mb Copper</p>
        <p>Module 6: 48x 100Mb Copper</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Purchase Date</td>
      <td style="text-align:left">Spring 2008</td>
    </tr>
  </tbody>
</table>## History

Core0 replaced an older Cisco 4006 switch running in hybrid mode \(CatOS on the SP and IOS on the RP\) as the core switch and router for the CSL. While it was being installed, a fire alarm went off; interrupting the install.

