/*
 * Licensed to the Apache Software Foundation (ASF) under one or more
 * contributor license agreements.  See the NOTICE file distributed with
 * this work for additional information regarding copyright ownership.
 * The ASF licenses this file to You under the Apache License, Version 2.0
 * (the "License"); you may not use this file except in compliance with
 * the License.  You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
package ewicket.wizard;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.OutputStreamWriter;
import java.lang.reflect.Method;

import org.eclipse.core.resources.IFile;
import org.eclipse.core.runtime.IPath;
import org.eclipse.jdt.core.IJavaElement;
import org.eclipse.jdt.internal.ui.wizards.NewClassCreationWizard;
import org.eclipse.jdt.ui.wizards.NewClassWizardPage;
import org.eclipse.jface.wizard.WizardPage;
import org.eclipse.ui.INewWizard;
import org.eclipse.wst.html.ui.internal.Logger;
import org.eclipse.wst.html.ui.internal.wizard.NewHTMLWizard;

/**
 * QWickieNewWebPageWizard, creates new Wicket WebPages as java class with corresponding html file
 * 
 * @author count.negative
 * 
 */
@SuppressWarnings("restriction")
public class EWicketNewWebPageWizard extends NewClassCreationWizard implements INewWizard {
	private final NewHTMLWizard htmlWiz = new NewHTMLWizard();
	
	public EWicketNewWebPageWizard() {
		setWindowTitle("Create a new Wicket Page");
		setHelpAvailable(false);
		setNeedsProgressMonitor(false);
	}

	@Override
	public void addPages() {
		super.addPages();
		NewClassWizardPage page = (NewClassWizardPage) getPage("NewClassWizardPage");
		page.setSuperClass("org.apache.wicket.markup.html.WebPage", true);
		page.setTitle(getWindowTitle());
		page.setDescription(getWindowTitle());
		htmlWiz.init(getWorkbench(), getSelection());
		htmlWiz.addPages();
		addPage(htmlWiz.getPage("NewHTMLTemplatesWizardPage"));
	}

	@Override
	public boolean performFinish() {
		final boolean finished = super.performFinish();
		if (finished) {
			final IJavaElement ce = getCreatedElement();
			final IPath p = ce.getResource().getProjectRelativePath().removeFileExtension().addFileExtension("html").makeAbsolute();
			final IPath pathP = ce.getResource().getProjectRelativePath().removeFileExtension().addFileExtension("properties").makeAbsolute();
			try {
				WizardPage nhtwp = (WizardPage) htmlWiz.getPage("NewHTMLTemplatesWizardPage");
				IFile file = ce.getJavaProject().getProject().getFile(p);
				IFile file_p = ce.getJavaProject().getProject().getFile(pathP);
				
				
				
				final Method m = nhtwp.getClass().getDeclaredMethod("getTemplateString", (Class<?>[]) null);
				m.setAccessible(true);
				final String templateString = (String) m.invoke(nhtwp, (Object[]) null);

				ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
				OutputStreamWriter outputStreamWriter = new OutputStreamWriter(outputStream);
				outputStreamWriter.write(templateString);
				outputStreamWriter.flush();
				outputStreamWriter.close();
				ByteArrayInputStream inputStream = new ByteArrayInputStream(outputStream.toByteArray());
				if (!file.exists()) {
					System.out.println("creo il file properties");
					file.create(inputStream, true, null);
				}
				inputStream.close();
				
				ByteArrayInputStream inputStream_p = new ByteArrayInputStream( new ByteArrayOutputStream().toByteArray());
				if (!file_p.exists()) {
					System.out.println("creo il file properties");
					file_p.create(inputStream_p, true, null);
				}
				inputStream_p.close();
				
				
			} catch (Exception e) {
				Logger.log(202, "Could not create contents for new html file", e);
			}
		}
		return finished;
	}
}