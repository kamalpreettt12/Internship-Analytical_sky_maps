# **Schechter Function Simulation and Sky Map Generation**

## **Overview**
This project simulates the distribution of flux sources following a Schechter luminosity function. It generates a histogram of sampled luminosities, plots the corresponding cumulative distribution function (CDF), and creates a 2D sky map with a Gaussian beam applied.

## **Installation**

To run this project, you'll need to have Python 3 installed along with the following libraries:

- `numpy`
- `scipy`
- `matplotlib`
- `astropy`

You can install these dependencies using pip:

```bash
pip install numpy scipy matplotlib astropy
```

In the python notebook, you will find codes:
1. To define a schechter function:
```bash
def schechter(S, N_star, s_star, alpha):
    return (N_star/s_star) * (S/s_star)**alpha * np.exp(-S/s_star)
```

2. Numerical integration to get the CDF
```bash
cdf = np.zeros_like(S)
for i, l in enumerate(S):
    cdf[i], _ = quad(schechter, 1e-2, l, args=(N_star, s_star, alpha))

3. Sampling the distirbution
```bash
uniform_random_samples = np.random.uniform(0, max(cdf), num_samples)
sampled_S_values = np.interp(uniform_random_samples, cdf, S)

4. Sky Map Creation and Smoothing
```bash
sky_map = np.zeros((sky_map_size, sky_map_size))

for i in range(num_samples):
    sky_map[ra_pixel_indices[i], dec_pixel_indices[i]] += sampled_S_values[i]

# Gaussian smoothing
smoothed_sky_map = gaussian_filter(sky_map, sigma=sigma)

5. Histogram Overlap and Comparison
```bash
plt.hist(sky_map.flatten(), bins=log_bins, color='blue', alpha=0.5, label='Original Map')
plt.hist(smoothed_sky_map.flatten(), bins=log_bins, color='red', alpha=0.5, label='Smoothed Map')
