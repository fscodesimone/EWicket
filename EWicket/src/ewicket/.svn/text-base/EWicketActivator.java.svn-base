package ewicket;

import org.eclipse.ui.plugin.AbstractUIPlugin;
import org.osgi.framework.BundleContext;

/**
 * The activator class controls the plug-in life cycle
 */
public class EWicketActivator extends AbstractUIPlugin {

	// The plug-in ID
	public static final String PLUGIN_ID = "EWicket";

	public static final String WICKET_ID = "wicket:id";
	public static final String WICKET_DTD = "http://wicket.apache.org";

	// The shared instance
	private static EWicketActivator plugin;

	/**
	 * The constructor
	 */
	public EWicketActivator() {
	}

	/*
	 * (non-Javadoc)
	 * 
	 * @see
	 * org.eclipse.ui.plugin.AbstractUIPlugin#start(org.osgi.framework.BundleContext
	 * )
	 */
	public void start(BundleContext context) throws Exception {
		super.start(context);
		plugin = this;
	}

	/*
	 * (non-Javadoc)
	 * 
	 * @see
	 * org.eclipse.ui.plugin.AbstractUIPlugin#stop(org.osgi.framework.BundleContext
	 * )
	 */
	public void stop(BundleContext context) throws Exception {
		plugin = null;
		super.stop(context);
	}

	/**
	 * Returns the shared instance
	 * 
	 * @return the shared instance
	 */
	public static EWicketActivator getDefault() {
		return plugin;
	}

}
